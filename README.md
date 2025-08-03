local Players = game:GetService("Players")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local Workspace = game:GetService("Workspace")
local RunService = game:GetService("RunService")
local LocalPlayer = Players.LocalPlayer

--// GUI: Tela de Carregamento
local loadingScreen = Instance.new("ScreenGui", game.CoreGui)
loadingScreen.Name = "MM2LoadingScreen"
local frame = Instance.new("Frame", loadingScreen)
frame.Size = UDim2.new(1,0,1,0)
frame.BackgroundColor3 = Color3.fromRGB(0,0,0)
local label = Instance.new("TextLabel", frame)
label.Size = UDim2.new(1,0,1,0)
label.Text = "Bem-vindo"
label.Font = Enum.Font.GothamSemibold
label.TextColor3 = Color3.new(1,1,1)
label.TextScaled = true
wait(3)
loadingScreen:Destroy()

--// GUI: Painel ESP
local gui = Instance.new("ScreenGui", game.CoreGui)
gui.Name = "MM2AutoFarmPanel"
local panel = Instance.new("Frame", gui)
panel.Position = UDim2.new(0.02,0,0.2,0)
panel.Size = UDim2.new(0,180,0,140)
panel.BackgroundColor3 = Color3.fromRGB(30,30,30)
panel.BorderSizePixel = 0

local title = Instance.new("TextLabel", panel)
title.Text = "MM2 AutoFarm"
title.Size = UDim2.new(1,0,0,28)
title.BackgroundTransparency = 1
title.TextColor3 = Color3.new(1,1,1)
title.Font = Enum.Font.GothamBold
title.TextSize = 20

local espToggle = Instance.new("TextButton", panel)
espToggle.Position = UDim2.new(0,10,0,40)
espToggle.Size = UDim2.new(0,160,0,30)
espToggle.Text = "Ativar ESP"
espToggle.Font = Enum.Font.GothamBold
espToggle.TextSize = 16
espToggle.BackgroundColor3 = Color3.fromRGB(60,60,80)
espToggle.TextColor3 = Color3.new(1,1,1)

local espActive = false

espToggle.MouseButton1Click:Connect(function()
    espActive = not espActive
    espToggle.Text = espActive and "Desativar ESP" or "Ativar ESP"
end)

--// ESP
local espObjects = {}
function clearESP()
    for _,v in pairs(espObjects) do
        if v.Adornee then v:Destroy() end
    end
    espObjects = {}
end

function createESP(player, color)
    pcall(function()
        if player ~= LocalPlayer and player.Character and player.Character:FindFirstChild("Head") then
            local bill = Instance.new("BillboardGui", gui)
            bill.Adornee = player.Character.Head
            bill.Size = UDim2.new(0,100,0,40)
            bill.StudsOffset = Vector3.new(0,2,0)
            bill.AlwaysOnTop = true
            local txt = Instance.new("TextLabel", bill)
            txt.Size = UDim2.new(1,0,1,0)
            txt.BackgroundTransparency = 1
            txt.Text = player.Name
            txt.TextColor3 = color
            txt.Font = Enum.Font.GothamBold
            txt.TextScaled = true
            table.insert(espObjects, bill)
        end
    end)
end

--// Função para identificar papéis (Assassino/Xerife)
function getRoles()
    local murderer, sheriff = nil, nil
    for _,plr in pairs(Players:GetPlayers()) do
        if plr.Backpack:FindFirstChild("Knife") or (plr.Character and plr.Character:FindFirstChild("Knife")) then
            murderer = plr
        elseif plr.Backpack:FindFirstChild("Gun") or (plr.Character and plr.Character:FindFirstChild("Gun")) then
            sheriff = plr
        end
    end
    return murderer, sheriff
end

--// Loop do ESP
RunService.RenderStepped:Connect(function()
    if espActive then
        clearESP()
        local murderer, sheriff = getRoles()
        if murderer then createESP(murderer, Color3.fromRGB(255,50,50)) end
        if sheriff then createESP(sheriff, Color3.fromRGB(50,150,255)) end
    else
        clearESP()
    end
end)

--// Auto Farm Coin
coroutine.wrap(function()
    while wait(0.5) do
        local coins = Workspace:FindFirstChild("Coins") or Workspace:FindFirstChild("CoinContainer")
        if coins then
            for _,coin in pairs(coins:GetChildren()) do
                if coin:IsA("BasePart") then
                    LocalPlayer.Character.HumanoidRootPart.CFrame = coin.CFrame + Vector3.new(0,2,0)
                    wait(0.15)
                end
            end
        end
    end
end)()

--// Auto Shoot (se for xerife)
coroutine.wrap(function()
    while wait(0.2) do
        local murderer, sheriff = getRoles()
        if sheriff == LocalPlayer and murderer and murderer.Character and murderer.Character:FindFirstChild("Head") then
            local gun = LocalPlayer.Backpack:FindFirstChild("Gun") or (LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("Gun"))
            if gun then
                -- Mira e atira no assassino
                local args = {
                    [1] = murderer.Character.Head.Position
                }
                -- Tenta atirar (depende do método do jogo, pode ser diferente)
                local remote = ReplicatedStorage:FindFirstChild("ShootGun") or ReplicatedStorage.Remotes:FindFirstChild("ShootGun")
                if remote and remote:IsA("RemoteEvent") then
                    remote:FireServer(unpack(args))
                end
            end
        end
    end
end)()

--// Hotkey para fechar painel
panel.Active = true
panel.Draggable = true
local closeBtn = Instance.new("TextButton", panel)
closeBtn.Position = UDim2.new(0,150,0,0)
closeBtn.Size = UDim2.new(0,28,0,28)
closeBtn.Text = "X"
closeBtn.TextColor3 = Color3.new(1,0.3,0.3)
closeBtn.TextSize = 16
closeBtn.BackgroundTransparency = 1
closeBtn.MouseButton1Click:Connect(function()
    gui:Destroy()
end)

-- FIMResizeBtn.TextColor3 = Color3.fromRGB(255,255,255)
ResizeBtn.Font = Enum.Font.GothamBold
ResizeBtn.TextSize = 22
ResizeBtn.Parent = Panel

local min = false
ResizeBtn.MouseButton1Click:Connect(function()
    if not min then
        Panel.Size = UDim2.new(0, 180, 0, 60)
        ResizeBtn.Position = UDim2.new(1, -40, 1, -40)
        min = true
    else
        Panel.Size = UDim2.new(0, 350, 0, 450)
        ResizeBtn.Position = UDim2.new(1, -40, 1, -40)
        min = false
    end
end)
