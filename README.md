local screenGui = Instance.new("ScreenGui")
screenGui.Name = "EspicFacePanel"
screenGui.Parent = game.Players.LocalPlayer:WaitForChild("PlayerGui")

local panel = Instance.new("Frame")
panel.Size = UDim2.new(0, 300, 0, 180)
panel.Position = UDim2.new(0.5, -150, 0.5, -90)
panel.BackgroundColor3 = Color3.fromRGB(60, 60, 80)
panel.BorderSizePixel = 0
panel.Name = "Panel"
panel.Active = true
panel.Draggable = true
panel.Parent = screenGui

-- Botão para fechar/abrir
local toggleBtn = Instance.new("TextButton")
toggleBtn.Size = UDim2.new(0, 30, 0, 30)
toggleBtn.Position = UDim2.new(1, -35, 0, 5)
toggleBtn.Text = "-"
toggleBtn.BackgroundColor3 = Color3.fromRGB(120, 60, 60)
toggleBtn.Parent = panel

-- Carinha Espic Face (exemplo)
local espicLabel = Instance.new("TextLabel")
espicLabel.Size = UDim2.new(0, 60, 0, 60)
espicLabel.Position = UDim2.new(0, 10, 0, 10)
espicLabel.Text = "😏" -- Espic Face
espicLabel.TextScaled = true
espicLabel.BackgroundTransparency = 1
espicLabel.Parent = panel

-- Função para criar botões
local function createButton(name, pos, callback)
    local btn = Instance.new("TextButton")
    btn.Size = UDim2.new(0, 110, 0, 32)
    btn.Position = UDim2.new(0, pos.X, 0, pos.Y)
    btn.Text = name
    btn.BackgroundColor3 = Color3.fromRGB(80, 80, 120)
    btn.TextColor3 = Color3.new(1,1,1)
    btn.Parent = panel
    btn.MouseButton1Click:Connect(callback)
    return btn
end

-- Botões e funções (exemplos)
local function esp() print("ESP ativado") end
local function unesp() print("ESP desativado") end
local function flw() print("Fly ativado") end
local function unfly() print("Fly desativado") end
local function view() print("View ativado") end
local function pleyer() print("Player selecionado") end
local function unview() print("Unview ativado") end
local function fling() print("Fling ativado") end
local function unfling() print("Fling desativado") end

createButton("ESP", Vector2.new(80, 10), esp)
createButton("UNESP", Vector2.new(80, 50), unesp)
createButton("FLW", Vector2.new(80, 90), flw)
createButton("UNFLY", Vector2.new(80, 130), unfly)
createButton("VIEW", Vector2.new(200, 10), view)
createButton("PLEYER", Vector2.new(200, 50), pleyer)
createButton("UNVIEW", Vector2.new(200, 90), unview)
createButton("FLING", Vector2.new(200, 130), fling)
createButton("UNFLING", Vector2.new(80, 170), unfling)

-- Mexer personagem sozinho (exemplo simples)
local function mexerPersonagem()
    local char = game.Players.LocalPlayer.Character
    if char and char:FindFirstChild("HumanoidRootPart") then
        char.HumanoidRootPart.CFrame = char.HumanoidRootPart.CFrame * CFrame.new(0,0,10)
    end
end

-- Exemplo: mover personagem ao clicar na carinha
espicLabel.MouseButton1Click:Connect(mexerPersonagem)

-- Abrir/fechar painel
local aberto = true
toggleBtn.MouseButton1Click:Connect(function()
    aberto = not aberto
    for _,v in ipairs(panel:GetChildren()) do
        if v ~= toggleBtn then
            v.Visible = aberto
        end
    end
    toggleBtn.Text = aberto and "-" or "+"
end)

-- Inicialmente aberto
for _,v in ipairs(panel:GetChildren()) do
    if v ~= toggleBtn then
        v.Visible = true
    end
end
