local Players = game:GetService("Players")
local StarterGui = game:GetService("StarterGui")
local LocalPlayer = Players.LocalPlayer

-- Função para clonar HumanoidDescription
local function copiarAvatar(jogadorAlvo)
    if jogadorAlvo and jogadorAlvo.Character and jogadorAlvo ~= LocalPlayer then
        local desc = Players:GetHumanoidDescriptionFromUserId(jogadorAlvo.UserId)
        if LocalPlayer.Character and LocalPlayer.Character:FindFirstChildOfClass("Humanoid") then
            LocalPlayer.Character:FindFirstChildOfClass("Humanoid"):ApplyDescription(desc)
        end
    end
end

-- Função para criar Epic Face
local function criarEpicFace(imagem)
    imagem.Image = "rbxassetid://7074786" -- ID Epic face
end

-- UI
local screenGui = Instance.new("ScreenGui")
screenGui.Name = "CopiarAvatarPanel"
screenGui.Parent = StarterGui

local painel = Instance.new("Frame")
painel.Size = UDim2.new(0, 240, 0, 120)
painel.Position = UDim2.new(0.35, 0, 0.3, 0)
painel.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
painel.Active = true
painel.Draggable = true
painel.Visible = true
painel.Parent = screenGui

-- Epic Face
local epicFace = Instance.new("ImageLabel")
epicFace.Size = UDim2.new(0, 60, 0, 60)
epicFace.Position = UDim2.new(0, 10, 0, 10)
epicFace.BackgroundTransparency = 1
criarEpicFace(epicFace)
epicFace.Parent = painel

-- Label de instrução
local label = Instance.new("TextLabel")
label.Size = UDim2.new(0, 140, 0, 40)
label.Position = UDim2.new(0, 80, 0, 10)
label.BackgroundTransparency = 1
label.Text = "Selecione um jogador e clique em copiar"
label.TextWrapped = true
label.TextColor3 = Color3.fromRGB(255, 255, 255)
label.Font = Enum.Font.GothamBold
label.TextSize = 14
label.Parent = painel

-- Dropdown de jogadores
local dropDown = Instance.new("TextButton")
dropDown.Size = UDim2.new(0, 120, 0, 30)
dropDown.Position = UDim2.new(0, 80, 0, 55)
dropDown.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
dropDown.Text = "Escolha um jogador"
dropDown.TextColor3 = Color3.new(1,1,1)
dropDown.Parent = painel

-- Botão copiar
local copiarBtn = Instance.new("TextButton")
copiarBtn.Size = UDim2.new(0, 80, 0, 30)
copiarBtn.Position = UDim2.new(0, 80, 0, 90)
copiarBtn.BackgroundColor3 = Color3.fromRGB(90, 170, 90)
copiarBtn.Text = "Copiar"
copiarBtn.TextColor3 = Color3.new(1,1,1)
copiarBtn.Font = Enum.Font.GothamBold
copiarBtn.TextSize = 16
copiarBtn.Parent = painel

-- Botão abrir/fechar
local openClose = Instance.new("TextButton")
openClose.Size = UDim2.new(0, 55, 0, 25)
openClose.Position = UDim2.new(1, -60, 0, -30)
openClose.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
openClose.Text = "Fechar"
openClose.TextColor3 = Color3.new(1,1,1)
openClose.Font = Enum.Font.GothamBold
openClose.TextSize = 14
openClose.Parent = painel

-- Lógica para abrir/fechar painel
openClose.MouseButton1Click:Connect(function()
    painel.Visible = false
    -- Botão para abrir painel
    local abrirBtn = Instance.new("TextButton")
    abrirBtn.Size = UDim2.new(0, 60, 0, 30)
    abrirBtn.Position = UDim2.new(0, 30, 0, 60)
    abrirBtn.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
    abrirBtn.Text = "Abrir"
    abrirBtn.TextColor3 = Color3.new(1,1,1)
    abrirBtn.Font = Enum.Font.GothamBold
    abrirBtn.TextSize = 16
    abrirBtn.Parent = screenGui
    abrirBtn.MouseButton1Click:Connect(function()
        painel.Visible = true
        abrirBtn:Destroy()
    end)
end)

-- Atualiza lista de jogadores no dropdown
local jogadorSelecionado = nil
dropDown.MouseButton1Click:Connect(function()
    local menu = Instance.new("Frame")
    menu.Size = UDim2.new(0,
    
