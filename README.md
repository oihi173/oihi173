--[[ 
  Script: Esp Colorido Unesp + Painel (abrir/fechar) com Catarina Epic Face e View/Unview
  Jogo: Roblox
  Autor: oihi173 (pedido via GitHub Copilot)
  Descrição:
    - ESP colorido para jogadores da Unesp
    - Painel GUI para abrir/fechar
    - Catarina Epic Face decorativo
    - Botão para View/Unview (espectar) um jogador
]]

-- Serviços
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local LocalPlayer = Players.LocalPlayer

-- Função utilitária para criar ESP colorido
local function createESP(target)
    local espBox = Instance.new("BoxHandleAdornment")
    espBox.Name = "UnespESP"
    espBox.Adornee = target.Character and target.Character:FindFirstChild("HumanoidRootPart")
    espBox.AlwaysOnTop = true
    espBox.ZIndex = 10
    espBox.Size = Vector3.new(3, 6, 3)
    espBox.Transparency = 0.5
    -- Cor do ESP colorido (arco-íris)
    local hue = tick() % 5 / 5
    espBox.Color3 = Color3.fromHSV(hue, 1, 1)
    espBox.Parent = target.Character
    return espBox
end

-- Atualiza e mantém o ESP nas personagens
local function updateESP()
    for _, plr in ipairs(Players:GetPlayers()) do
        if plr ~= LocalPlayer and plr.Team and plr.Team.Name:lower():find("unesp") then
            if plr.Character and plr.Character:FindFirstChild("HumanoidRootPart") then
                if not plr.Character:FindFirstChild("UnespESP") then
                    createESP(plr)
                else
                    local espBox = plr.Character:FindFirstChild("UnespESP")
                    local hue = tick() % 5 / 5
                    espBox.Color3 = Color3.fromHSV(hue, 1, 1)
                end
            end
        end
    end
end

-- Remove ESPs de jogadores que não são mais Unesp
local function cleanupESP()
    for _, plr in ipairs(Players:GetPlayers()) do
        if plr.Character and plr.Character:FindFirstChild("UnespESP") then
            if not (plr.Team and plr.Team.Name:lower():find("unesp")) then
                plr.Character.UnespESP:Destroy()
            end
        end
    end
end

-- UI Painel
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "UnespPainel"
ScreenGui.Parent = game.CoreGui or LocalPlayer.PlayerGui

local MainFrame = Instance.new("Frame")
MainFrame.Size = UDim2.new(0, 300, 0, 180)
MainFrame.Position = UDim2.new(0, 50, 0, 100)
MainFrame.BackgroundColor3 = Color3.fromRGB(32,32,32)
MainFrame.BorderSizePixel = 2
MainFrame.Visible = true
MainFrame.Parent = ScreenGui

local ToggleButton = Instance.new("TextButton")
ToggleButton.Size = UDim2.new(0, 120, 0, 32)
ToggleButton.Position = UDim2.new(0, 10, 0, 10)
ToggleButton.Text = "Fechar Painel"
ToggleButton.BackgroundColor3 = Color3.fromRGB(68, 68, 200)
ToggleButton.TextColor3 = Color3.new(1,1,1)
ToggleButton.Font = Enum.Font.GothamBold
ToggleButton.TextSize = 18
ToggleButton.Parent = MainFrame

-- Catarina Epic Face
local Face = Instance.new("ImageLabel")
Face.Size = UDim2.new(0, 60, 0, 60)
Face.Position = UDim2.new(0, 220, 0, 10)
Face.BackgroundTransparency = 1
Face.Image = "rbxassetid://8523660790" -- Epic Face
Face.Parent = MainFrame
local CatarinaText = Instance.new("TextLabel")
CatarinaText.Size = UDim2.new(0, 60, 0, 16)
CatarinaText.Position = UDim2.new(0, 220, 0, 70)
CatarinaText.BackgroundTransparency = 1
CatarinaText.Text = "Catarina"
CatarinaText.Font = Enum.Font.GothamBold
CatarinaText.TextColor3 = Color3.new(1,1,0)
CatarinaText.TextSize = 14
CatarinaText.Parent = MainFrame

-- Dropdown de jogadores Unesp
local PlayerList = Instance.new("TextLabel")
PlayerList.Size = UDim2.new(0, 260, 0, 20)
PlayerList.Position = UDim2.new(0, 10, 0, 55)
PlayerList.BackgroundTransparency = 1
PlayerList.Text = "Selecione um jogador Unesp para View/Unview:"
PlayerList.Font = Enum.Font.Gotham
PlayerList.TextColor3 = Color3.new(1,1,1)
PlayerList.TextSize = 14
PlayerList.Parent = MainFrame

local PlayerDrop = Instance.new("TextButton")
PlayerDrop.Size = UDim2.new(0, 180, 0, 28)
PlayerDrop.Position = UDim2.new(0, 10, 0, 80)
PlayerDrop.BackgroundColor3 = Color3.fromRGB(99, 99, 160)
PlayerDrop.Text = "Selecionar Jogador"
PlayerDrop.TextColor3 = Color3.new(1,1,1)
PlayerDrop.Font = Enum.Font.Gotham
PlayerDrop.TextSize = 16
PlayerDrop.Parent = MainFrame

local SelectedPlayer = nil

-- View/Unview buttons
local ViewBtn = Instance.new("TextButton")
ViewBtn.Size = UDim2.new(0, 80, 0, 28)
ViewBtn.Position = UDim2.new(0, 200, 0, 80)
ViewBtn.Text = "View"
ViewBtn.BackgroundColor3 = Color3.fromRGB(0, 150, 0)
ViewBtn.TextColor3 = Color3.new(1,1,1)
ViewBtn.Font = Enum.Font.GothamBold
ViewBtn.TextSize = 16
ViewBtn.Parent = MainFrame

local UnviewBtn = Instance.new("TextButton")
UnviewBtn.Size = UDim2.new(0, 80, 0, 28)
UnviewBtn.Position = UDim2.new(0, 200, 0, 115)
UnviewBtn.Text = "Unview"
UnviewBtn.BackgroundColor3 = Color3.fromRGB(200, 0, 0)
UnviewBtn.TextColor3 = Color3.new(1,1,1)
UnviewBtn.Font = Enum.Font.GothamBold
UnviewBtn.TextSize = 16
UnviewBtn.Parent = MainFrame

-- Função para atualizar a lista de jogadores Unesp
local function getUnespPlayers()
    local uns = {}
    for _, plr in ipairs(Players:GetPlayers()) do
        if plr ~= LocalPlayer and plr.Team and plr.Team.Name:lower():find("unesp") then
            table.insert(uns, plr)
        end
    end
    return uns
end

-- Menu de seleção simples (popup)
local function showPlayerMenu()
    local menu = Instance.new("Frame")
    menu.Size = UDim2.new(0, 180, 0, 24 * #getUnespPlayers())
    menu.Position = PlayerDrop.Position + UDim2.new(0, 0, 0, 28)
    menu.BackgroundColor3 = Color3.fromRGB(44,44,80)
    menu.BorderSizePixel = 1
    menu.ZIndex = 50
    menu.Parent = MainFrame
    
    local uns = getUnespPlayers()
    for i, plr in ipairs(uns) do
        local btn = Instance.new("TextButton")
        btn.Size = UDim2.new(1, 0, 0, 24)
        btn.Position = UDim2.new(0, 0, 0, (i-1)*24)
        btn.Text = plr.Name
        btn.BackgroundColor3 = Color3.fromRGB(99,99,160)
        btn.TextColor3 = Color3.new(1,1,1)
        btn.Font = Enum.Font.Gotham
        btn.TextSize = 14
        btn.ZIndex = 51
        btn.Parent = menu
        btn.MouseButton1Click:Connect(function()
            SelectedPlayer = plr
            PlayerDrop.Text = plr.Name
            menu:Destroy()
        end)
    end
end
PlayerDrop.MouseButton1Click:Connect(showPlayerMenu)

-- View: Camera segue o jogador selecionado
ViewBtn.MouseButton1Click:Connect(function()
    if SelectedPlayer and SelectedPlayer.Character and SelectedPlayer.Character:FindFirstChild("HumanoidRootPart") then
        workspace.CurrentCamera.CameraSubject = SelectedPlayer.Character.Humanoid
    end
end)
-- Unview: Camera volta para o jogador local
UnviewBtn.MouseButton1Click:Connect(function()
    if LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("Humanoid") then
        workspace.CurrentCamera.CameraSubject = LocalPlayer.Character.Humanoid
    end
end)

-- Painel abrir/fechar
ToggleButton.MouseButton1Click:Connect(function()
    MainFrame.Visible = not MainFrame.Visible
    ToggleButton.Text = MainFrame.Visible and "Fechar Painel" or "Abrir Painel"
end)

-- Loop de ESP
RunService.RenderStepped:Connect(function()
    updateESP()
    cleanupESP()
end)

-- Pronto!
print("[UNESP PAINEL] Script carregado! Painel aberto no canto superior esquerdo.")

-- DICA: Caso o painel não apareça, tente rodar o script no executor com interface habilitada.
