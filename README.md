local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "RedzHub"
ScreenGui.Parent = game.Players.LocalPlayer:WaitForChild("PlayerGui")

-- Criando o Frame principal
local MainFrame = Instance.new("Frame")
MainFrame.Size = UDim2.new(0, 400, 0, 420)
MainFrame.Position = UDim2.new(0.3, 0, 0.2, 0)
MainFrame.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
MainFrame.BorderSizePixel = 0
MainFrame.Parent = ScreenGui

-- Título
local Title = Instance.new("TextLabel")
Title.Size = UDim2.new(1, 0, 0, 40)
Title.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
Title.Text = "REDz HUB v3.5 : Brookhaven RP"
Title.TextColor3 = Color3.fromRGB(255, 255, 255)
Title.Font = Enum.Font.GothamBold
Title.TextSize = 16
Title.Parent = MainFrame

-- Lista lateral (abas)
local Tabs = {"início", "Casas", "Carros", "Trolar", "Teleportes", "Itens do avatar", "Itens", "Visual/Cliente", "Segredos", "outros"}

local UIListLayout = Instance.new("UIListLayout")

local SideMenu = Instance.new("Frame")
SideMenu.Size = UDim2.new(0, 110, 1, -40)
SideMenu.Position = UDim2.new(0, 0, 0, 40)
SideMenu.BackgroundColor3 = Color3.fromRGB(35, 35, 35)
SideMenu.Parent = MainFrame

UIListLayout.Parent = SideMenu
UIListLayout.SortOrder = Enum.SortOrder.LayoutOrder

for _,tab in pairs(Tabs) do
    local Button = Instance.new("TextButton")
    Button.Size = UDim2.new(1, 0, 0, 30)
    Button.Text = tab
    Button.TextColor3 = Color3.fromRGB(255,255,255)
    Button.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
    Button.Font = Enum.Font.Gotham
    Button.TextSize = 14
    Button.Parent = SideMenu
end

-- Área de conteúdo
local ContentFrame = Instance.new("Frame")
ContentFrame.Size = UDim2.new(1, -110, 1, -40)
ContentFrame.Position = UDim2.new(0, 110, 0, 40)
ContentFrame.BackgroundColor3 = Color3.fromRGB(
