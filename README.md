local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local mouse = LocalPlayer:GetMouse()

local panel = Instance.new("ScreenGui")
panel.Name = "PainelCompleto"
panel.Parent = game:GetService("CoreGui")

local mainFrame = Instance.new("Frame")
mainFrame.Size = UDim2.new(0, 350, 0, 400)
mainFrame.Position = UDim2.new(0.5, -175, 0.5, -200)
mainFrame.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
mainFrame.BorderSizePixel = 2
mainFrame.Active = true
mainFrame.Draggable = true
mainFrame.Parent = panel

local capa = Instance.new("TextLabel")
capa.Size = UDim2.new(1, 0, 0, 210)
capa.Position = UDim2.new(0,0,0,0)
capa.BackgroundTransparency = 1
capa.TextColor3 = Color3.fromRGB(200,255,200)
capa.Font = Enum.Font.Code
capa.TextSize = 16
capa.TextXAlignment = Enum.TextXAlignment.Left
capa.TextYAlignment = Enum.TextYAlignment.Top
capa.Text = [[⠀⠀⠀⠀⠀⠀⠀⠀⠀⣀⣠⣤⠶⠶⠶⠶⠶⢦⣤⣀⡀⠀⠀⠀⠀⠀⠀⠀⠀⠀
⠀⠀⠀⠀⠀⠀⣠⡶⠛⠋⠀⠀⠀⠀⠀⠀⠀⠀⠀⠈⠙⠛⢦⣄⠀⠀⠀⠀⠀⠀
⠀⠀⠀⠀⣰⠞⠁⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠉⠳⣄⠀⠀⠀⠀
⠀⠀⢠⡾⠁⠀⠀⢀⣤⣤⣤⣤⣄⠀⠀⠀⠀⠀⠀⠀⠀⣠⣤⣤⣤⡙⢷⡀⠀⠀
⠀⢠⡟⠀⠀⠀⣰⣿⣿⡇⠀⠀⠙⢷⡀⠀⠀⠀⠀⢠⣾⣿⣿⠀⠀⠹⣮⢿⡀⠀
⠀⣾⠀⠀⠀⢰⡏⠙⠛⠁⠀⠀⠀⠘⡇⠀⠀⠀⠀⣸⠋⠛⠋⠀⠀⠀⢹⠈⣧⠀
⢸⡇⠀⠀⠀⠘⣧⣤⣤⣤⣤⣤⣤⣤⡇⠀⠀⠀⠀⢸⣧⣤⣤⣤⣤⣤⣾⠀⢹⡆
⢸⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⢸⡇
⢸⡇⠀⠀⠀⠀⣰⣶⣶⣶⣶⣶⣶⣶⣶⣶⣶⣶⣶⣶⣶⣶⣶⣶⡶⠀⠀⣸⠇
⠀⣿⠀⠀⠀⠀⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⠁⠀⢀⡟⠀
⠀⠘⣧⠀⠀⠀⢻⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⠃⠀⠀⣾⠁⠀
⠀⠀⠘⢷⡀⠀⠀⢻⣿⡻⠋⠁⠀⠀⠀⠉⠻⣿⣿⣿⣿⡿⠃⠀⢠⡾⠁⠀⠀
⠀⠀⠀⠀⠻⣦⡀⠀⠙⠷⣤⣀⠀⠀⠀⠀⠀⠈⣿⣿⡿⠋⠀⣀⡴⠋⠀⠀⠀⠀
⠀⠀⠀⠀⠀⠀⠙⠷⣤⣀⠀⠉⠛⠓⠒⠒⠒⠚⠋⠁⣠⣤⠾⠋⠀⠀⠀⠀⠀⠀
⠀⠀⠀⠀⠀⠀⠀⠀⠀⠉⠙⠛⠶⠶⠶⠶⠶⠶⠛⠋⠉⠀⠀⠀⠀⠀⠀⠀⠀⠀]]
capa.Parent = mainFrame

local closeBtn = Instance.new("TextButton")
closeBtn.Size = UDim2.new(0, 40, 0, 30)
closeBtn.Position = UDim2.new(1, -45, 0, 5)
closeBtn.Text = "✕"
closeBtn.BackgroundColor3 = Color3.fromRGB(200, 50, 50)
closeBtn.TextColor3 = Color3.fromRGB(255,255,255)
closeBtn.Parent = mainFrame

local openBtn = Instance.new("TextButton")
openBtn.Size = UDim2.new(0, 60, 0, 30)
openBtn.Position = UDim2.new(0, 10, 1, -40)
openBtn.Text = "Abrir Painel"
openBtn.BackgroundColor3 = Color3.fromRGB(50, 200, 50)
openBtn.TextColor3 = Color3.fromRGB(255,255,255)
openBtn.Visible = false
openBtn.Parent = panel

local moveLeftBtn = Instance.new("TextButton")
moveLeftBtn.Size = UDim2.new(0, 40, 0, 30)
moveLeftBtn.Position = UDim2.new(0, 5, 0, 5)
moveLeftBtn.Text = "←"
moveLeftBtn.Parent = mainFrame

local moveRightBtn = Instance.new("TextButton")
moveRightBtn.Size = UDim2.new(0, 40, 0, 30)
moveRightBtn.Position = UDim2.new(0, 55, 0, 5)
moveRightBtn.Text = "→"
moveRightBtn.Parent = mainFrame

-- Botão utilitário
local function createButton(text, posY)
    local btn = Instance.new("TextButton")
    btn.Size = UDim2.new(0.8, 0, 0, 30)
    btn.Position = UDim2.new(0.1, 0, 0, posY)
    btn.BackgroundColor3 = Color3.fromRGB(40, 40, 60)
    btn.TextColor3 = Color3.fromRGB(255,255,255)
    btn.Text = text
    btn.Font = Enum.Font.GothamBold
    btn.TextSize = 16
    btn.Parent = mainFrame
    return btn
end

local btnEsp = createButton("ESP", 220)
local btnUnEsp = createButton("UnESP", 255)
local btnFly = createButton("Fly", 290)
local btnUnFly = createButton("UnFly", 325)
local btnView = createButton("View Player", 360)
local btnUnView = createButton("UnView", 395)
local btnFling = createButton("Fling Player", 430)
local btnUnFling = createButton("UnFling Player", 465)
local btnIA = createButton("IA Movement", 500)

-- Fechar/Abrir Painel
closeBtn.MouseButton1Click:Connect(function()
    mainFrame.Visible = false
    openBtn.Visible = true
end)
openBtn.MouseButton1Click:Connect(function()
    mainFrame.Visible = true
    openBtn.Visible = false
end)

-- Mover para os lados
moveLeftBtn.MouseButton1Click:Connect(function()
    local pos = mainFrame.Position
    mainFrame.Position = UDim2.new(pos.X.Scale, pos.X.Offset-50, pos.Y.Scale, pos.Y.Offset)
end)
moveRightBtn.MouseButton1Click:Connect(function()
    local pos = mainFrame.Position
    mainFrame.Position = UDim2.new(pos.X.Scale, pos.X.Offset+50, pos.Y.Scale, pos.Y.Offset)
end)

-- Funções dos botões
local espOn = false
btnEsp.MouseButton1Click:Connect(function()
    espOn = true
    for _, p in pairs(Players:GetPlayers()) do
        if p ~= LocalPlayer and p.Character and p.Character:FindFirstChild("Head") then
            local billboard = Instance.new("BillboardGui")
            billboard.Name = "ESP"
            billboard.Adornee = p.Character.Head
            billboard.Size = UDim2.new(0, 100, 0, 40)
            billboard.AlwaysOnTop = true
            billboard.Parent = p.Character.Head
            local label = Instance.new("TextLabel")
            label.Size = UDim2.new(1,0,1,0)
            label.BackgroundTransparency = 1
            label.Text = p.Name
            label.TextColor3 = Color3.fromRGB(255,50,50)
            label.TextScaled = true
            label.Parent = billboard
        end
    end
end)
btnUnEsp.MouseButton1Click:Connect(function()
    espOn = false
    for _, p in pairs(Players:GetPlayers()) do
        if p.Character and p.Character:FindFirstChild("Head") then
            local esp = p.Character.Head:FindFirstChild("ESP")
            if esp then esp:Destroy() end
        end
    end
end)

local flying = false
local flyConn
btnFly.MouseButton1Click:Connect(function()
    if flying then return end
    flying = true
    local char = LocalPlayer.Character
    if not char or not char:FindFirstChild("HumanoidRootPart") then return end
    flyConn = game:GetService("RunService").Heartbeat:Connect(function()
        char.HumanoidRootPart.Velocity = Vector3.new(0, 60, 0)
    end)
end)
btnUnFly.MouseButton1Click:Connect(function()
    flying = false
    if flyConn then flyConn:Disconnect() end
end)

local viewing = false
local origCam
btnView.MouseButton1Click:Connect(function()
    local target = Players:GetPlayers()[2]
    if not target or not target.Character or not target.Character:FindFirstChild("Head") then return end
    origCam = workspace.CurrentCamera.CameraSubject
    workspace.CurrentCamera.CameraSubject = target.Character.Head
    viewing = true
end)
btnUnView.MouseButton1Click:Connect(function()
    if viewing and origCam then
        workspace.CurrentCamera.CameraSubject = origCam
        viewing = false
    end
end)

local flinging = false
local flingConn
btnFling.MouseButton1Click:Connect(function()
    local target = Players:GetPlayers()[2]
    if not target or not target.Character or not target.Character:FindFirstChild("HumanoidRootPart") then return end
    flinging = true
    flingConn = game:GetService("RunService").Stepped:Connect(function()
        target.Character.HumanoidRootPart.Velocity = Vector3.new(500, 500, 500)
    end)
end)
btnUnFling.MouseButton1Click:Connect(function()
    flinging = false
    if flingConn then flingConn:Disconnect() end
end)

-- IA que controla o boneco sozinho
local iaActive = false
local iaConn
btnIA.MouseButton1Click:Connect(function()
    iaActive = not iaActive
    btnIA.Text = iaActive and "Parar IA" or "IA Movement"
    if iaActive then
        local directions = {Vector3.new(1,0,0), Vector3.new(-1,0,0), Vector3.new(0,0,1), Vector3.new(0,0,-1)}
        iaConn = game:GetService("RunService").Heartbeat:Connect(function()
            local char = LocalPlayer.Character
            if char and char:FindFirstChild("HumanoidRootPart") then
                local dir = directions[math.random(1,#directions)]
                char.Humanoid:Move(dir, true)
            end
        end)
    else
        if iaConn then iaConn:Disconnect() end
        btnIA.Text = "IA Movement"
    end
end)
