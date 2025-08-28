-- LocalScript em StarterGui

local player = game.Players.LocalPlayer
local gui = Instance.new("ScreenGui", player:WaitForChild("PlayerGui"))
gui.Name = "WildHorseIslandsScriptV2"
gui.ResetOnSpawn = false

-- Botão de abrir/fechar
local toggleButton = Instance.new("TextButton")
toggleButton.Size = UDim2.new(0, 100, 0, 40)
toggleButton.Position = UDim2.new(0, 10, 0.8, 0)
toggleButton.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
toggleButton.Text = "Menu"
toggleButton.TextColor3 = Color3.fromRGB(255, 255, 255)
toggleButton.Font = Enum.Font.SourceSansBold
toggleButton.TextSize = 18
toggleButton.Parent = gui

-- Painel principal
local mainFrame = Instance.new("Frame")
mainFrame.Size = UDim2.new(0, 350, 0, 400)
mainFrame.Position = UDim2.new(0.3, 0, 0.2, 0)
mainFrame.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
mainFrame.BorderSizePixel = 0
mainFrame.Visible = false
mainFrame.Parent = gui

-- Arrastar painel
local dragging, dragInput, dragStart, startPos

local function update(input)
	local delta = input.Position - dragStart
	mainFrame.Position = UDim2.new(
		startPos.X.Scale, startPos.X.Offset + delta.X,
		startPos.Y.Scale, startPos.Y.Offset + delta.Y
	)
end

mainFrame.InputBegan:Connect(function(input)
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

mainFrame.InputChanged:Connect(function(input)
	if input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch then
		dragInput = input
	end
end)

game:GetService("UserInputService").InputChanged:Connect(function(input)
	if input == dragInput and dragging then
		update(input)
	end
end)

-- Título
local title = Instance.new("TextLabel")
title.Size = UDim2.new(1, 0, 0, 35)
title.BackgroundColor3 = Color3.fromRGB(35, 35, 35)
title.Text = "Wild Horse Islands Script V2 | By VIPER"
title.TextColor3 = Color3.fromRGB(255, 255, 255)
title.Font = Enum.Font.SourceSansBold
title.TextSize = 16
title.Parent = mainFrame

-- Container abas
local tabFrame = Instance.new("Frame")
tabFrame.Size = UDim2.new(0, 90, 1, -35)
tabFrame.Position = UDim2.new(0, 0, 0, 35)
tabFrame.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
tabFrame.Parent = mainFrame

-- Conteúdo das abas
local contentFrame = Instance.new("Frame")
contentFrame.Size = UDim2.new(1, -90, 1, -35)
contentFrame.Position = UDim2.new(0, 90, 0, 35)
contentFrame.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
contentFrame.Parent = mainFrame

-- Função checkbox
local function createToggle(parent, text, order)
	local button = Instance.new("TextButton")
	button.Size = UDim2.new(1, -20, 0, 25)
	button.Position = UDim2.new(0, 10, 0, 10 + (order * 30))
	button.BackgroundColor3 = Color3.fromRGB(45, 45, 45)
	button.Text = "[ ] " .. text
	button.TextColor3 = Color3.fromRGB(255, 255, 255)
	button.Font = Enum.Font.SourceSans
	button.TextSize = 14
	button.Parent = parent

	local enabled = false
	button.MouseButton1Click:Connect(function()
		enabled = not enabled
		if enabled then
			button.Text = "[X] " .. text
		else
			button.Text = "[ ] " .. text
		end
	end)
end

-- Criar abas
local tabs = {}
local function createTab(name)
	local tabButton = Instance.new("TextButton")
	tabButton.Size = UDim2.new(1, 0, 0, 30)
	tabButton.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
	tabButton.Text = name
	tabButton.TextColor3 = Color3.fromRGB(255, 255, 255)
	tabButton.Font = Enum.Font.SourceSansBold
	tabButton.TextSize = 14
	tabButton.Parent = tabFrame

	local tabContent = Instance.new("Frame")
	tabContent.Size = UDim2.new(1, 0, 1, 0)
	tabContent.BackgroundTransparency = 1
	tabContent.Visible = false
	tabContent.Parent = contentFrame

	tabs[name] = {Button = tabButton, Content = tabContent}

	tabButton.MouseButton1Click:Connect(function()
		for _, v in pairs(tabs) do
			v.Content.Visible = false
			v.Button.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
		end
		tabContent.Visible = true
		tabButton.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
	end)

	return tabContent
end

-- Criando abas
local mainTab = createTab("Main")
local itemModsTab = createTab("Item Mods")
local espTab = createTab("ESP/Chams")
local charTab = createTab("Character")
local miscTab = createTab("Misc")

-- Opções
createToggle(itemModsTab, "Reduce Lasso Cooldown", 0)
createToggle(itemModsTab, "Expand Capture Radius", 1)
createToggle(itemModsTab, "Disable Travel Cost", 2)
createToggle(itemModsTab, "Disable Level Requirement", 3)

createToggle(espTab, "Enable ESP", 0)
createToggle(espTab, "Show Horses Only", 1)

createToggle(charTab, "Infinite Stamina", 0)
createToggle(charTab, "Speed Boost", 1)

createToggle(miscTab, "Auto Collect", 0)
createToggle(miscTab, "Disable Fog", 1)

-- Aba inicial
tabs["Item Mods"].Content.Visible = true
tabs["Item Mods"].Button.BackgroundColor3 = Color3.fromRGB(60, 60, 60)

-- Botão abre/fecha
toggleButton.MouseButton1Click:Connect(function()
	mainFrame.Visible = not mainFrame.Visible
end)
