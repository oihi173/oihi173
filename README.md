local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local RunService = game:GetService("RunService")
local TweenService = game:GetService("TweenService")

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
local trollFace2 = trollFace:Clone()
trollFace2.Parent = mainFrame
trollFace2.Position = UDim2.new(0.5, -50, 0, 10)
local title2 = title:Clone()
title2.Parent = mainFrame
title2.Position = UDim2.new(0, 0, 0, 120)

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

local y = 170
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
    txt.Text = player.DisplayName or player.Name
    txt.TextColor3 = color
    txt.Font = Enum.Font.FredokaOne
    txt.TextStrokeTransparency = 0.5
    txt.TextSize = 16
    table.insert(espObjects, box)
end

-- Detecção de papéis
local function GetRole(p)
    local backpack = p:FindFirstChild("Backpack") or p:FindFirstChildOfClass("Backpack")
    if backpack then
        if backpack:FindFirstChild("Knife") then return "Assassino" end
        if backpack:FindFirstChild("Gun") then return "Xerife" end
    end
    local char = p.Character
    if char then
        if char:FindFirstChild("Knife") then return "Assassino" end
        if char:FindFirstChild("Gun") then return "Xerife" end
    end
    return "Inocente"
end

-- Loop ESP
spawn(function()
    while true do
        RemoveAllESP()
        for _,p in pairs(Players:GetPlayers()) do
            local role = GetRole(p)
            if states.esp_murder and role == "Assassino" then AddESP(p, Color3.new(1,0,0)) end
            if states.esp_sheriff and role == "Xerife" then AddESP(p, Color3.new(0,0,1)) end
            if states.esp_innocent and role == "Inocente" then AddESP(p, Color3.new(0,1,0)) end
        end
        wait(0.5)
    end
end)

-- Auto Farm: Noclip + voar até moeda
local function Noclip()
    local char = LocalPlayer.Character
    if char then
        for _,v in pairs(char:GetChildren()) do
            if v:IsA("BasePart") then
                v.CanCollide = false
            end
        end
    end
end

local function GetNearestCoin()
    local char = LocalPlayer.Character
    if not char or not char:FindFirstChild("HumanoidRootPart") then return nil end
    local pos = char.HumanoidRootPart.Position
    local nearest, dist = nil, math.huge
    for _,v in pairs(workspace:GetDescendants()) do
        if v.Name == "Coin" and v:IsA("BasePart") and v.Transparency < 1 then
            local d = (v.Position - pos).magnitude
            if d < dist then
                nearest = v
                dist = d
            end
        end
    end
    return nearest
end

spawn(function()
    while true do
        if states.autofarm then
            Noclip()
            local char = LocalPlayer.Character
            local root = char and char:FindFirstChild("HumanoidRootPart")
            local coin = GetNearestCoin()
            if root and coin then
                -- Voa para moeda
                root.CFrame = root.CFrame:Lerp(CFrame.new(coin.Position + Vector3.new(0,3,0)), 0.25)
            end
        end
        wait(0.1)
    end
end)

-- Auto Shot (exemplo básico)
spawn(function()
    while true do
        if states.autoshoot then
            local char = LocalPlayer.Character
            if char and char:FindFirstChild("Gun") then
                for _,p in pairs(Players:GetPlayers()) do
                    if p ~= LocalPlayer and GetRole(p) == "Assassino" then
                        -- Tenta mirar e atirar (depende do executor e API)
                        -- Exemplo: char.Gun:FireServer(p.Character.HumanoidRootPart.Position)
                    end
                end
            end
        end
        wait(0.4)
    end
end)
