-- Painel com Fly, View Players e ESP (LocalScript)

local Players = game:GetService("Players")
local UIS = game:GetService("UserInputService")
local player = Players.LocalPlayer

-- Criando a interface
local screenGui = Instance.new("ScreenGui", player.PlayerGui)
screenGui.Name = "FlyESPPanel"

local frame = Instance.new("Frame", screenGui)
frame.Position = UDim2.new(0, 20, 0, 100)
frame.Size = UDim2.new(0, 200, 0, 240)
frame.BackgroundColor3 = Color3.fromRGB(30,30,30)
frame.BackgroundTransparency = 0.2

local uiListLayout = Instance.new("UIListLayout", frame)
uiListLayout.Padding = UDim.new(0,10)

local function addButton(name)
    local btn = Instance.new("TextButton", frame)
    btn.Size = UDim2.new(1, -20, 0, 40)
    btn.Position = UDim2.new(0,10,0,0)
    btn.Text = name
    btn.BackgroundColor3 = Color3.fromRGB(50,50,50)
    btn.TextColor3 = Color3.new(1,1,1)
    btn.Font = Enum.Font.SourceSansBold
    btn.TextSize = 22
    btn.AutoButtonColor = true
    return btn
end

local flyBtn = addButton("Fly: OFF")
local viewPlayersBtn = addButton("View Players")
local espBtn = addButton("ESP: OFF")

-- FLY FUNCTIONALITY
local flying = false
local speed = 50
local bodyGyro, bodyVelocity

local function fly()
    local char = player.Character or player.CharacterAdded:Wait()
    local root = char:WaitForChild("HumanoidRootPart")
    bodyGyro = Instance.new("BodyGyro", root)
    bodyGyro.P = 9e4
    bodyGyro.MaxTorque = Vector3.new(9e9,9e9,9e9)
    bodyGyro.CFrame = root.CFrame

    bodyVelocity = Instance.new("BodyVelocity", root)
    bodyVelocity.MaxForce = Vector3.new(9e9,9e9,9e9)
    bodyVelocity.Velocity = Vector3.new(0,0,0)

    flying = true
    flyBtn.Text = "Fly: ON"

    while flying do
        local cam = workspace.CurrentCamera
        local moveDir = Vector3.new()
        if UIS:IsKeyDown(Enum.KeyCode.W) then moveDir = moveDir + cam.CFrame.LookVector end
        if UIS:IsKeyDown(Enum.KeyCode.S) then moveDir = moveDir - cam.CFrame.LookVector end
        if UIS:IsKeyDown(Enum.KeyCode.A) then moveDir = moveDir - cam.CFrame.RightVector end
        if UIS:IsKeyDown(Enum.KeyCode.D) then moveDir = moveDir + cam.CFrame.RightVector end
        bodyVelocity.Velocity = moveDir.Magnitude > 0 and moveDir.Unit * speed or Vector3.new()
        bodyGyro.CFrame = cam.CFrame
        game:GetService("RunService").RenderStepped:Wait()
    end
    if bodyGyro then bodyGyro:Destroy() end
    if bodyVelocity then bodyVelocity:Destroy() end
    flyBtn.Text = "Fly: OFF"
end

local function stopFly()
    flying = false
end

flyBtn.MouseButton1Click:Connect(function()
    if not flying then
        fly()
    else
        stopFly()
    end
end)

-- VIEW PLAYERS FUNCTIONALITY
viewPlayersBtn.MouseButton1Click:Connect(function()
    local playerList = {}
    for _,plr in ipairs(Players:GetPlayers()) do
        table.insert(playerList, plr.Name)
    end
    local msg = "Jogadores:\n" .. table.concat(playerList, "\n")
    local popup = Instance.new("TextLabel", screenGui)
    popup.Size = UDim2.new(0,200,0,200)
    popup.Position = UDim2.new(0,230,0,100)
    popup.BackgroundColor3 = Color3.fromRGB(40,40,40)
    popup.TextColor3 = Color3.new(1,1,1)
    popup.Text = msg
    popup.Font = Enum.Font.SourceSans
    popup.TextSize = 20
    popup.TextWrapped = true
    popup.TextYAlignment = Enum.TextYAlignment.Top
    popup.ZIndex = 10
    popup.Name = "PlayersPopup"

    wait(3)
    if popup then popup:Destroy() end
end)

-- ESP FUNCTIONALITY
local espOn = false
local espObjects = {}

local function addEsp(plr)
    if plr == player then return end
    local char = plr.Character
    if not char then return end
    local head = char:FindFirstChild("Head")
    if not head then return end

    local billboard = Instance.new("BillboardGui", head)
    billboard.Name = "ESPBillboard"
    billboard.Size = UDim2.new(0,100,0,40)
    billboard.AlwaysOnTop = true

    local label = Instance.new("TextLabel", billboard)
    label.Size = UDim2.new(1,0,1,0)
    label.BackgroundTransparency = 1
    label.Text = plr.Name
    label.TextColor3 = Color3.new(1,0,0)
    label.Font = Enum.Font.SourceSansBold
    label.TextSize = 18
    table.insert(espObjects, billboard)
end

local function removeEsp()
    for _,esp in ipairs(espObjects) do
        if esp and esp.Parent then esp:Destroy() end
    end
    espObjects = {}
end

espBtn.MouseButton1Click:Connect(function()
    espOn = not espOn
    espBtn.Text = espOn and "ESP: ON" or "ESP: OFF"
    removeEsp()
    if espOn then
        for _,plr in ipairs(Players:GetPlayers()) do
            addEsp(plr)
        end
    end
end)

Players.PlayerAdded:Connect(function(plr)
    if espOn then
        plr.CharacterAdded:Connect(function()
            wait(1)
            addEsp(plr)
        end)
    end
end)
Players.PlayerRemoving:Connect(function(plr)
    removeEsp()
    if espOn then
        for _,plr in ipairs(Players:GetPlayers()) do
            addEsp(plr)
        end
    end
end)

-- FIM DO SCRIPT
