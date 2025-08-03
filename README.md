local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local RunService = game:GetService("RunService")
local TweenService = game:GetService("TweenService")
local UIS = game:GetService("UserInputService")

-- Função para criar GUI
local function CreateGuiElement(class, props)
    local obj = Instance.new(class)
    for k, v in pairs(props or {}) do
        obj[k] = v
    end
    return obj
end

-- Remove GUIs antigos para evitar duplicatas
pcall(function() if LocalPlayer.PlayerGui:FindFirstChild("TrollFacePanel") then LocalPlayer.PlayerGui.TrollFacePanel:Destroy() end end)

-- Tela de carregamento
local screenGui = CreateGuiElement("ScreenGui", {Name = "TrollFacePanel", ResetOnSpawn = false, Parent = LocalPlayer:WaitForChild("PlayerGui")})
local loadingFrame = CreateGuiElement("Frame", {
    Parent = screenGui,
    Size = UDim2.new(0, 400, 0, 300),
    Position = UDim2.new(0.5, -200, 0.5, -150),
    BackgroundColor3 = Color3.fromRGB(40, 40, 40),
    BorderSizePixel = 0,
    Visible = true,
})
local trollFace = CreateGuiElement("ImageLabel", {
    Parent = loadingFrame,
    Size = UDim2.new(0, 100, 0, 100),
    Position = UDim2.new(0.5, -50, 0, 10),
    BackgroundTransparency = 1,
    Image = "http://www.roblox.com/asset/?id=518009006", -- Troll face asset
})
local title = CreateGuiElement("TextLabel", {
    Parent = loadingFrame,
    Size = UDim2.new(1, 0, 0, 30),
    Position = UDim2.new(0, 0, 0, 120),
    BackgroundTransparency = 1,
    Text = "Troll Face by oihi_173",
    TextColor3 = Color3.fromRGB(255, 255, 255),
    Font = Enum.Font.FredokaOne,
    TextSize = 28,
})
local countLabel = CreateGuiElement("TextLabel", {
    Parent = loadingFrame,
    Size = UDim2.new(1, 0, 0, 60),
    Position = UDim2.new(0, 0, 0, 160),
    BackgroundTransparency = 1,
    Text = "Carregando...",
    TextColor3 = Color3.fromRGB(255,255,0),
    Font = Enum.Font.FredokaOne,
    TextSize = 36,
})

-- Tela de carregamento animada (1 a 7)
for i = 1, 7 do
    countLabel.Text = "Carregando... " .. tostring(i)
    wait(1)
end

loadingFrame.Visible = false

-- Painel principal
local mainFrame = CreateGuiElement("Frame", {
    Parent = screenGui,
    Size = UDim2.new(0, 410, 0, 350),
    Position = UDim2.new(0.5, -205, 0.5, -175),
    BackgroundColor3 = Color3.fromRGB(30, 30, 30),
    BorderSizePixel = 5,
    BorderColor3 = Color3.fromRGB(255,255,0),
    Visible = true,
})
mainFrame.Active = true
mainFrame.Draggable = true

-- Painel de movimentação lateral
local dragBar = CreateGuiElement("Frame", {
    Parent = mainFrame,
    BackgroundTransparency = 1,
    Size = UDim2.new(1,0,0,30),
    Position = UDim2.new(0,0,0,0),
    Name = "DragBar"
})

local closeButton = CreateGuiElement("TextButton", {
    Parent = dragBar,
    Size = UDim2.new(0,30,0,30),
    Position = UDim2.new(1,-30,0,0),
    BackgroundColor3 = Color3.fromRGB(255,60,60),
    Text = "X",
    TextColor3 = Color3.new(1,1,1),
    Font = Enum.Font.FredokaOne,
    TextSize = 20,
    BorderSizePixel = 0,
    Name = "CloseBtn"
})

local openButton = CreateGuiElement("TextButton", {
    Parent = screenGui,
    Size = UDim2.new(0, 50, 0, 40),
    Position = UDim2.new(0, 10, 0.5, -20),
    BackgroundColor3 = Color3.fromRGB(255, 255, 0),
    Text = ">",
    TextColor3 = Color3.new(0,0,0),
    Font = Enum.Font.FredokaOne,
    TextSize = 30,
    BorderSizePixel = 0,
    Visible = false,
    Name = "OpenBtn"
})

local isOpen = true

-- Abrir/fechar painel com animação
local function togglePanel()
    if isOpen then
        isOpen = false
        TweenService:Create(mainFrame, TweenInfo.new(0.3, Enum.EasingStyle.Quad, Enum.EasingDirection.Out), {
            Position = UDim2.new(0, -420, 0.5, -175)
        }):Play()
        wait(0.32)
        mainFrame.Visible = false
        openButton.Visible = true
    else
        openButton.Visible = false
        mainFrame.Visible = true
        TweenService:Create(mainFrame, TweenInfo.new(0.3, Enum.EasingStyle.Quad, Enum.EasingDirection.Out), {
            Position = UDim2.new(0.5, -205, 0.5, -175)
        }):Play()
        isOpen = true
    end
end

closeButton.MouseButton1Click:Connect(togglePanel)
openButton.MouseButton1Click:Connect(togglePanel)

-- Movimento lateral com setas
local moveAmount = 50
UIS.InputBegan:Connect(function(input, gpe)
    if gpe then return end
    if not isOpen then return end
    if input.KeyCode == Enum.KeyCode.Right then
        local newX = mainFrame.Position.X.Offset + moveAmount
        TweenService:Create(mainFrame, TweenInfo.new(0.2), {Position = UDim2.new(mainFrame.Position.X.Scale, math.clamp(newX, -205, workspace.CurrentCamera.ViewportSize.X-410), mainFrame.Position.Y.Scale, mainFrame.Position.Y.Offset)}):Play()
    elseif input.KeyCode == Enum.KeyCode.Left then
        local newX = mainFrame.Position.X.Offset - moveAmount
        TweenService:Create(mainFrame, TweenInfo.new(0.2), {Position = UDim2.new(mainFrame.Position.X.Scale, math.clamp(newX, 0, workspace.CurrentCamera.ViewportSize.X-410), mainFrame.Position.Y.Scale, mainFrame.Position.Y.Offset)}):Play()
    end
end)

-- Drag manual (opcional: já está ativo/draggable, mas para mobile pode fazer assim)
local dragging, dragInput, dragStart, startPos
dragBar.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
        dragging = true
        dragStart = input.Position
        startPos = mainFrame.Position
        input.Changed:Connect(function()
            if input.UserInputState == Enum.UserInputState.End then
                dragging = false
            end
        end)
    end
end)
dragBar.InputChanged:Connect(function(input)
    if dragging and (input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch) then
        local delta = input.Position - dragStart
        mainFrame.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
    end
end)

-- Conteúdo do painel
local trollFace2 = trollFace:Clone()
trollFace2.Parent = mainFrame
trollFace2.Position = UDim2.new(0.5, -50, 0, 40)
local title2 = title:Clone()
title2.Parent = mainFrame
title2.Position = UDim2.new(0, 0, 0, 150)

-- Botões
local options = {
    {Name="Auto Farm", Key="autofarm"}, 
    {Name="Auto Shot", Key="autoshoot"},
    {Name="ESP Xerife", Key="esp_sheriff"},
    {Name="ESP Assassino", Key="esp_murder"},
    {Name="ESP Inocente", Key="esp_innocent"},
    {Name="Desativar ESP", Key="esp_off"},
}
local states = {
    autofarm = false,
    autoshoot = false,
    esp_sheriff = false,
    esp_murder = false,
    esp_innocent = false
}

local y = 200
for i, opt in ipairs(options) do
    local btn = CreateGuiElement("TextButton", {
        Parent = mainFrame,
        Size = UDim2.new(0, 250, 0, 35),
        Position = UDim2.new(0.5,-125,0, y),
        BackgroundColor3 = Color3.fromRGB(60,60,60),
        Text = opt.Name,
        TextColor3 = Color3.fromRGB(255,255,255),
        Font = Enum.Font.FredokaOne,
        TextSize = 22,
        Name = opt.Key,
        BorderSizePixel = 2,
        BorderColor3 = Color3.fromRGB(255,255,0),
    })
    btn.MouseButton1Click:Connect(function()
        if opt.Key == "esp_off" then
            states.esp_sheriff = false
            states.esp_murder = false
            states.esp_innocent = false
            RemoveAllESP()
        elseif opt.Key == "autofarm" then
            states.autofarm = not states.autofarm
            btn.BackgroundColor3 = states.autofarm and Color3.fromRGB(0, 200, 0) or Color3.fromRGB(60,60,60)
        elseif opt.Key == "autoshoot" then
            states.autoshoot = not states.autoshoot
            btn.BackgroundColor3 = states.autoshoot and Color3.fromRGB(0, 200, 0) or Color3.fromRGB(60,60,60)
        else
            local key = opt.Key
            states[key] = not states[key]
            btn.BackgroundColor3 = states[key] and Color3.fromRGB(0, 200, 0) or Color3.fromRGB(60,60,60)
        end
    end)
    y = y + 40
end

-- Música após 7 segundos
local function PlayMusic()
    local sound = Instance.new("Sound", mainFrame)
    sound.SoundId = "rbxassetid://1843525033" -- Meme song: "Troll Song"
    sound.Volume = 1
    sound.Looped = true
    wait(0.2)
    sound:Play()
end
spawn(function()
    wait(7)
    PlayMusic()
end)

-- ESP
local espObjects = {}

function RemoveAllESP()
    for _,v in pairs(espObjects) do
        if v and v.Adornee and v.Parent then v:Destroy() end
    end
    espObjects = {}
end

function AddESP(player, color)
    if player == LocalPlayer then return end
    local char = player.Character
    if not char or not char:FindFirstChild("HumanoidRootPart") then return end
    local box = Instance.new("BillboardGui", mainFrame)
    box.Adornee = char.HumanoidRootPart
    box.AlwaysOnTop = true
    box.Size = UDim2.new(0,50,0,20)
    local txt = Instance.new("TextLabel", box)
    txt.Size = UDim2.new(1,0,1,0)
    txt.BackgroundTransparency = 1
    txt.Text = player.Name
    txt.TextColor3 = color or Color3.new(1,1,1)
    txt.Font = Enum.Font.FredokaOne
    txt.TextSize = 18
    box.Parent = mainFrame
    table.insert(espObjects, box)
end

-- Atalho: abrir/fechar com tecla F (opcional)
UIS.InputBegan:Connect(function(input, gpe)
    if gpe then return end
    if input.KeyCode == Enum.KeyCode.F then
        togglePanel()
    end
end)

-- Botão de abrir aparece após fechar
mainFrame:GetPropertyChangedSignal("Visible"):Connect(function()
    if not mainFrame.Visible then
        openButton.Visible = true
    end
end)
