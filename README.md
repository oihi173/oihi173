local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local RunService = game:GetService("RunService")
local StarterGui = game:GetService("StarterGui")

-- Criação do painel UI
local screenGui = Instance.new("ScreenGui")
screenGui.Name = "ESPPanel"
screenGui.Parent = LocalPlayer:WaitForChild("PlayerGui")

local panel = Instance.new("Frame")
panel.Size = UDim2.new(0, 320, 0, 160)
panel.Position = UDim2.new(0, 40, 0, 40)
panel.BackgroundColor3 = Color3.fromRGB(24, 24, 24)
panel.BorderSizePixel = 0
panel.Visible = true
panel.Active = true -- permite drag
panel.Draggable = false -- custom drag para só horizontal
panel.Parent = screenGui

local title = Instance.new("TextLabel")
title.Size = UDim2.new(1, 0, 0, 32)
title.Position = UDim2.new(0, 0, 0, 0)
title.Text = "Painel ESP Unesp"
title.Font = Enum.Font.GothamBold
title.TextColor3 = Color3.fromRGB(255,255,255)
title.BackgroundTransparency = 1
title.TextSize = 20
title.Parent = panel

local toggleBtn = Instance.new("TextButton")
toggleBtn.Size = UDim2.new(0, 80, 0, 28)
toggleBtn.Position = UDim2.new(1, -90, 0, 8)
toggleBtn.Text = "Fechar"
toggleBtn.Font = Enum.Font.GothamSemibold
toggleBtn.TextSize = 15
toggleBtn.BackgroundColor3 = Color3.fromRGB(44, 44, 44)
toggleBtn.TextColor3 = Color3.fromRGB(255,255,255)
toggleBtn.Parent = panel

local msgLabel = Instance.new("TextLabel")
msgLabel.Size = UDim2.new(1, -20, 0, 32)
msgLabel.Position = UDim2.new(0, 10, 0, 38)
msgLabel.Text = ""
msgLabel.Font = Enum.Font.GothamSemibold
msgLabel.TextColor3 = Color3.fromRGB(200, 255, 200)
msgLabel.BackgroundTransparency = 1
msgLabel.TextSize = 15
msgLabel.Parent = panel

local espStatus = Instance.new("TextLabel")
espStatus.Size = UDim2.new(1, -20, 0, 24)
espStatus.Position = UDim2.new(0, 10, 0, 70)
espStatus.Text = "ESP: Desativado"
espStatus.Font = Enum.Font.Gotham
espStatus.TextColor3 = Color3.fromRGB(255,255,0)
espStatus.BackgroundTransparency = 1
espStatus.TextSize = 14
espStatus.Parent = panel

local espBtn = Instance.new("TextButton")
espBtn.Size = UDim2.new(0, 120, 0, 28)
espBtn.Position = UDim2.new(0, 10, 0, 100)
espBtn.Text = "Ativar ESP Unesp"
espBtn.Font = Enum.Font.GothamSemibold
espBtn.TextSize = 15
espBtn.BackgroundColor3 = Color3.fromRGB(44, 44, 44)
espBtn.TextColor3 = Color3.fromRGB(255,255,255)
espBtn.Parent = panel

-- Arrastar o painel lateralmente
local dragging = false
local dragStart = nil
local panelStart = nil

panel.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 then
        dragging = true
        dragStart = input.Position
        panelStart = panel.Position
    end
end)
panel.InputEnded:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 then
        dragging = false
    end
end)
game:GetService("UserInputService").InputChanged:Connect(function(input)
    if dragging and input.UserInputType == Enum.UserInputType.MouseMovement then
        local delta = input.Position - dragStart
        -- Só mover X (horizontal)
        panel.Position = UDim2.new(panelStart.X.Scale, panelStart.X.Offset + delta.X, panelStart.Y.Scale, panelStart.Y.Offset)
    end
end)

-- Abrir/Fechar painel
toggleBtn.MouseButton1Click:Connect(function()
    panel.Visible = not panel.Visible
    if panel.Visible then
        toggleBtn.Text = "Fechar"
    else
        toggleBtn.Text = "Abrir"
    end
end)

-- Mensagem animada ao ativar
local function animateMsg(txt)
    msgLabel.Text = ""
    for i = 1, #txt do
        msgLabel.Text = string.sub(txt, 1, i)
        wait(0.03)
    end
end

-- ESP Quadrado
local espActive = false
local boxAdorns = {}

local function clearESP()
    for _, adorn in pairs(boxAdorns) do
        adorn:Destroy()
    end
    boxAdorns = {}
end

local function updateESP()
    clearESP()
    for _, player in ipairs(Players:GetPlayers()) do
        if player ~= LocalPlayer and player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
            local root = player.Character.HumanoidRootPart
            local box = Instance.new("BoxHandleAdornment")
            box.Adornee = root
            box.Size = Vector3.new(4, 7, 2) -- Tamanho hitbox padrão
            box.Color3 = Color3.fromRGB(0, 255, 0)
            box.Transparency = 0.5
            box.ZIndex = 10
            box.AlwaysOnTop = true
            box.Parent = root
            table.insert(boxAdorns, box)
        end
    end
end

-- Loop de ESP
RunService.RenderStepped:Connect(function()
    if espActive then
        updateESP()
    else
        clearESP()
    end
end)

espBtn.MouseButton1Click:Connect(function()
    espActive = not espActive
    if espActive then
        espStatus.Text = "ESP: Ativado"
        espStatus.TextColor3 = Color3.fromRGB(0,255,0)
        espBtn.Text = "Desativar ESP"
        animateMsg("este *script foi feito e alguns coisas que eu tive pensando*")
    else
        espStatus.Text = "ESP: Desativado"
        espStatus.TextColor3 = Color3.fromRGB(255,255,0)
        espBtn.Text = "Ativar ESP Unesp"
        msgLabel.Text = ""
        clearESP()
    end
end)
