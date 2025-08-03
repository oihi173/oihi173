local gui = Instance.new("ScreenGui")
gui.Name = "PainelMM2"
gui.ResetOnSpawn = false
gui.Parent = game.CoreGui

local welcomeFrame = Instance.new("Frame", gui)
welcomeFrame.Size = UDim2.new(0, 400, 0, 120)
welcomeFrame.Position = UDim2.new(0.5, -200, 0.5, -60)
welcomeFrame.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
welcomeFrame.BorderSizePixel = 0

local welcomeText = Instance.new("TextLabel", welcomeFrame)
welcomeText.Size = UDim2.new(1, 0, 1, 0)
welcomeText.BackgroundTransparency = 1
welcomeText.Text = "Bem-vindo\nScripts tst"
welcomeText.TextColor3 = Color3.fromRGB(255,255,255)
welcomeText.Font = Enum.Font.SourceSansBold
welcomeText.TextScaled = true

wait(7)
welcomeFrame:Destroy()

-- // FUNÇÃO DRAG (PAINEL MÓVEL)
local function dragify(Frame)
    local dragging, dragInput, dragStart, startPos
    Frame.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
            dragging = true
            dragStart = input.Position
            startPos = Frame.Position
            input.Changed:Connect(function()
                if input.UserInputState == Enum.UserInputState.End then dragging = false end
            end)
        end
    end)
    Frame.InputChanged:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch then
            dragInput = input
        end
    end)
    game:GetService("UserInputService").InputChanged:Connect(function(input)
        if input == dragInput and dragging then
            local delta = input.Position - dragStart
            Frame.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
        end
    end)
end

-- // PAINEL PRINCIPAL
local mainFrame = Instance.new("Frame", gui)
mainFrame.Size = UDim2.new(0, 340, 0, 340)
mainFrame.Position = UDim2.new(0.5, -170, 0.5, -170)
mainFrame.BackgroundColor3 = Color3.fromRGB(27,27,27)
mainFrame.BorderSizePixel = 0
mainFrame.Visible = true

dragify(mainFrame)

-- // BOTÃO FECHAR/ABRIR
local openBtn = Instance.new("TextButton", gui)
openBtn.Size = UDim2.new(0, 80, 0, 35)
openBtn.Position = UDim2.new(0, 10, 0.5, -20)
openBtn.BackgroundColor3 = Color3.fromRGB(40,40,40)
openBtn.Text = "Abrir Painel"
openBtn.TextColor3 = Color3.fromRGB(255,255,255)
openBtn.Visible = false

local closeBtn = Instance.new("TextButton", mainFrame)
closeBtn.Size = UDim2.new(0, 30, 0, 30)
closeBtn.Position = UDim2.new(1, -35, 0, 5)
closeBtn.BackgroundColor3 = Color3.fromRGB(80,0,0)
closeBtn.Text = "X"
closeBtn.TextColor3 = Color3.fromRGB(255,255,255)

closeBtn.MouseButton1Click:Connect(function()
    mainFrame.Visible = false
    openBtn.Visible = true
end)
openBtn.MouseButton1Click:Connect(function()
    mainFrame.Visible = true
    openBtn.Visible = false
end)

-- // TÍTULO E CRÉDITO
local titulo = Instance.new("TextLabel", mainFrame)
titulo.Size = UDim2.new(1, 0, 0, 36)
titulo.Position = UDim2.new(0, 0, 0, 0)
titulo.BackgroundTransparency = 1
titulo.Text = "Painel MM2 | by oihi_173"
titulo.Font = Enum.Font.SourceSansBold
titulo.TextSize = 22
titulo.TextColor3 = Color3.fromRGB(0,255,127)

-- // CRIAÇÃO DAS OPÇÕES
local function criarBotao(txt, ordem)
    local btn = Instance.new("TextButton", mainFrame)
    btn.Size = UDim2.new(1, -40, 0, 36)
    btn.Position = UDim2.new(0, 20, 0, 50 + (ordem-1)*44)
    btn.BackgroundColor3 = Color3.fromRGB(40,40,40)
    btn.Text = txt
    btn.Font = Enum.Font.SourceSansBold
    btn.TextSize = 19
    btn.TextColor3 = Color3.fromRGB(255,255,255)
    btn.Name = txt:gsub(" ", "")
    return btn
end

local autoFarmBtn = criarBotao("Auto Farm", 1)
local unautoFarmBtn = criarBotao("UnAuto Farm", 2)
local autoShotBtn = criarBotao("Auto Shot Murder", 3)
local espAssassinBtn = criarBotao("ESP Assassino/Xerife", 4)
local espInocenteBtn = criarBotao("ESP Inocente", 5)
local farmFuncBtn = criarBotao("Farm Funcional (Ir até moedas)", 6)

-- // FUNÇÕES CORE
local autoFarmAtivo = false
local noclipCon = nil

function ativarNoclip()
    if noclipCon then noclipCon:Disconnect() end
    noclipCon = game:GetService("RunService").Stepped:Connect(function()
        pcall(function()
            local char = game.Players.LocalPlayer.Character
            if char then
                for _,v in pairs(char:GetDescendants()) do
                    if v:IsA("BasePart") then v.CanCollide = false end
                end
            end
        end)
    end)
end
function desativarNoclip()
    if noclipCon then noclipCon:Disconnect() end
end

function autoFarm()
    autoFarmAtivo = true
    ativarNoclip()
    while autoFarmAtivo and wait(0.5) do
        local char = game.Players.LocalPlayer.Character
        if not char then continue end
        local hrp = char:FindFirstChild("HumanoidRootPart")
        if not hrp then continue end
        -- Coletar moedas (bolas amarelas)
        for _,obj in pairs(workspace:GetChildren()) do
            if obj.Name == "CoinContainer" or obj.Name == "Coin" then
                for _,coin in pairs(obj:GetChildren()) do
                    if coin:IsA("BasePart") then
                        hrp.CFrame = coin.CFrame + Vector3.new(0,2,0)
                        wait(0.1)
                    end
                end
            end
        end
    end
    desativarNoclip()
end

function unAutoFarm()
    autoFarmAtivo = false
    desativarNoclip()
end

autoFarmBtn.MouseButton1Click:Connect(function()
    if not autoFarmAtivo then
        spawn(autoFarm)
    end
end)
unautoFarmBtn.MouseButton1Click:Connect(unAutoFarm)

farmFuncBtn.MouseButton1Click:Connect(function()
    -- Mesma lógica do autoFarm, mas pode ser expandida para só coletar moedas
    if not autoFarmAtivo then
        spawn(autoFarm)
    end
end)

-- // ESP FUNÇÕES
local highlightFolder = Instance.new("Folder", gui)
highlightFolder.Name = "ESPFolder"

function limparESP()
    for _,v in pairs(highlightFolder:GetChildren()) do v:Destroy() end
end

function criarESP(cor, player)
    pcall(function()
        if player and player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
            local hl = Instance.new("Highlight", highlightFolder)
            hl.Adornee = player.Character
            hl.FillColor = cor
            hl.OutlineColor = Color3.fromRGB(0,0,0)
            hl.FillTransparency = 0.5
            hl.OutlineTransparency = 0
            hl.Name = player.Name .. "_ESP"
        end
    end)
end

espAssassinBtn.MouseButton1Click:Connect(function()
    limparESP()
    for _,p in pairs(game.Players:GetPlayers()) do
        if p ~= game.Players.LocalPlayer and p.Character then
            local tool = p.Backpack:FindFirstChild("Knife") or (p.Character:FindFirstChildOfClass("Tool") and p.Character:FindFirstChildOfClass("Tool").Name == "Knife")
            local sheriff = p.Backpack:FindFirstChild("Gun") or (p.Character:FindFirstChildOfClass("Tool") and p.Character:FindFirstChildOfClass("Tool").Name == "Gun")
            if tool then
                criarESP(Color3.fromRGB(255,0,0), p) -- Assassino
            elseif sheriff then
                criarESP(Color3.fromRGB(0,0,255), p) -- Xerife
            end
        end
    end
end)

espInocenteBtn.MouseButton1Click:Connect(function()
    limparESP()
    for _,p in pairs(game.Players:GetPlayers()) do
        if p ~= game.Players.LocalPlayer and p.Character then
            local tool = p.Backpack:FindFirstChild("Knife") or (p.Character:FindFirstChildOfClass("Tool") and p.Character:FindFirstChildOfClass("Tool").Name == "Knife")
            local sheriff = p.Backpack:FindFirstChild("Gun") or (p.Character:FindFirstChildOfClass("Tool") and p.Character:FindFirstChildOfClass("Tool").Name == "Gun")
            if not tool and not sheriff then
                criarESP(Color3.fromRGB(0,255,0), p) -- Inocente
            end
        end
    end
end)

autoShotBtn.MouseButton1Click:Connect(function()
    -- Tenta atirar automaticamente no assassino se for xerife
    local lp = game.Players.LocalPlayer
    local char = lp.Character
    if not char then return end
    local tool = char:FindFirstChildOfClass("Tool")
    if tool and tool.Name == "Gun" then
        for _,p in pairs(game.Players:GetPlayers()) do
            if p ~= lp then
                local isMurder = p.Backpack:FindFirstChild("Knife") or (p.Character and p.Character:FindFirstChildOfClass("Tool") and p.Character:FindFirstChildOfClass("Tool").Name == "Knife")
                if isMurder and p.Character and p.Character:FindFirstChild("HumanoidRootPart") then
                    -- Mirar e atirar
                    char.HumanoidRootPart.CFrame = CFrame.new(char.HumanoidRootPart.Position, p.Character.HumanoidRootPart.Position)
                    tool:Activate()
                end
            end
        end
    end
end)

-- // HOTKEY PARA ABRIR/FECHAR (tecla: F)
game:GetService("UserInputService").InputBegan:Connect(function(input, gp)
    if not gp and input.KeyCode == Enum.KeyCode.F then
        mainFrame.Visible = not mainFrame.Visible
        openBtn.Visible = not mainFrame.Visible
    end
end)

-- // ATUALIZAÇÃO DINÂMICA DO ESP (a cada 3s)
spawn(function()
    while true do
        wait(3)
        if highlightFolder:FindFirstChildWhichIsA("Highlight") then
            -- Atualiza ESP caso players mudem de personagem
            for _,hl in pairs(highlightFolder:GetChildren()) do
                if hl:IsA("Highlight") then
                    local pName = hl.Name:gsub("_ESP","")
                    local p = game.Players:FindFirstChild(pName)
                    if p and p.Character then
                        hl
                        
