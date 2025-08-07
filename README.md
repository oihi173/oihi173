local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "PainelMM2"
ScreenGui.Parent = game.CoreGui

local MainFrame = Instance.new("Frame")
MainFrame.Size = UDim2.new(0,350,0,320)
MainFrame.Position = UDim2.new(0,100,0,100)
MainFrame.BackgroundColor3 = Color3.fromRGB(40,40,40)
MainFrame.BorderSizePixel = 2
MainFrame.BorderColor3 = Color3.fromRGB(9,90,255)
MainFrame.Active = true
MainFrame.Draggable = true
MainFrame.Parent = ScreenGui

local Header = Instance.new("TextLabel")
Header.Size = UDim2.new(1,0,0,36)
Header.Position = UDim2.new(0,0,0,0)
Header.BackgroundColor3 = Color3.fromRGB(9,90,255)
Header.BorderSizePixel = 0
Header.Text = "Painel by oihi_173"
Header.Font = Enum.Font.SourceSansBold
Header.TextColor3 = Color3.new(1,1,1)
Header.TextSize = 20
Header.Parent = MainFrame

local CloseBtn = Instance.new("TextButton")
CloseBtn.Size = UDim2.new(0,28,0,28)
CloseBtn.Position = UDim2.new(1,-36,0,4)
CloseBtn.BackgroundColor3 = Color3.fromRGB(255,255,255)
CloseBtn.Text = "X"
CloseBtn.TextColor3 = Color3.fromRGB(9,90,255)
CloseBtn.Font = Enum.Font.SourceSansBold
CloseBtn.TextSize = 18
CloseBtn.BorderSizePixel = 0
CloseBtn.Parent = MainFrame

-- Abrir/Fechar painel
local OpenBtn = Instance.new("TextButton")
OpenBtn.Size = UDim2.new(0,120,0,32)
OpenBtn.Position = UDim2.new(0,10,0,10)
OpenBtn.BackgroundColor3 = Color3.fromRGB(9,90,255)
OpenBtn.Text = "Abrir Painel"
OpenBtn.TextColor3 = Color3.new(1,1,1)
OpenBtn.Font = Enum.Font.SourceSansBold
OpenBtn.TextSize = 16
OpenBtn.BorderSizePixel = 0
OpenBtn.Parent = ScreenGui
MainFrame.Visible = false

OpenBtn.MouseButton1Click:Connect(function()
    MainFrame.Visible = true
    OpenBtn.Visible = false
end)
CloseBtn.MouseButton1Click:Connect(function()
    MainFrame.Visible = false
    OpenBtn.Visible = true
end)

-- Seção do jogo
local GameLabel = Instance.new("TextLabel")
GameLabel.Size = UDim2.new(0.7,0,0,28)
GameLabel.Position = UDim2.new(0,14,0,46)
GameLabel.BackgroundTransparency = 1
GameLabel.Text = "Jogando:"
GameLabel.TextColor3 = Color3.fromRGB(255,255,255)
GameLabel.Font = Enum.Font.SourceSans
GameLabel.TextSize = 18
GameLabel.Parent = MainFrame

local UnespBtn = Instance.new("TextButton")
UnespBtn.Size = UDim2.new(0,70,0,26)
UnespBtn.Position = UDim2.new(0,110,0,46)
UnespBtn.BackgroundColor3 = Color3.fromRGB(9,90,255)
UnespBtn.Text = "Unesp"
UnespBtn.TextColor3 = Color3.new(1,1,1)
UnespBtn.Font = Enum.Font.SourceSansBold
UnespBtn.TextSize = 16
UnespBtn.BorderSizePixel = 0
UnespBtn.Parent = MainFrame

local MM2Btn = Instance.new("TextButton")
MM2Btn.Size = UDim2.new(0,70,0,26)
MM2Btn.Position = UDim2.new(0,190,0,46)
MM2Btn.BackgroundColor3 = Color3.fromRGB(9,90,255)
MM2Btn.Text = "MM2"
MM2Btn.TextColor3 = Color3.new(1,1,1)
MM2Btn.Font = Enum.Font.SourceSansBold
MM2Btn.TextSize = 16
MM2Btn.BorderSizePixel = 0
MM2Btn.Parent = MainFrame

local GameNowLabel = Instance.new("TextLabel")
GameNowLabel.Size = UDim2.new(0,100,0,26)
GameNowLabel.Position = UDim2.new(0,270,0,46)
GameNowLabel.BackgroundTransparency = 1
GameNowLabel.Text = "Nenhum"
GameNowLabel.TextColor3 = Color3.fromRGB(0,255,170)
GameNowLabel.Font = Enum.Font.SourceSansBold
GameNowLabel.TextSize = 15
GameNowLabel.Parent = MainFrame

UnespBtn.MouseButton1Click:Connect(function()
    GameNowLabel.Text = "Unesp"
    ESPSheriff.Text = "Sheriff: ???"
    ESPAssassin.Text = "Assassino: ???"
end)
MM2Btn.MouseButton1Click:Connect(function()
    GameNowLabel.Text = "MM2"
    ESPSheriff.Text = "Sheriff: Lucas"
    ESPAssassin.Text = "Assassino: Maria"
end)

-- ESP Section
local ESPLabel = Instance.new("TextLabel")
ESPLabel.Size = UDim2.new(0.7,0,0,28)
ESPLabel.Position = UDim2.new(0,14,0,82)
ESPLabel.BackgroundTransparency = 1
ESPLabel.Text = "ESP:"
ESPLabel.TextColor3 = Color3.fromRGB(255,255,255)
ESPLabel.Font = Enum.Font.SourceSans
ESPLabel.TextSize = 18
ESPLabel.Parent = MainFrame

local ESPBtn = Instance.new("TextButton")
ESPBtn.Size = UDim2.new(0,110,0,26)
ESPBtn.Position = UDim2.new(0,110,0,82)
ESPBtn.BackgroundColor3 = Color3.fromRGB(9,90,255)
ESPBtn.Text = "Ver Jogador"
ESPBtn.TextColor3 = Color3.new(1,1,1)
ESPBtn.Font = Enum.Font.SourceSansBold
ESPBtn.TextSize = 16
ESPBtn.BorderSizePixel = 0
ESPBtn.Parent = MainFrame

local UnviewBtn = Instance.new("TextButton")
UnviewBtn.Size = UDim2.new(0,70,0,26)
UnviewBtn.Position = UDim2.new(0,230,0,82)
UnviewBtn.BackgroundColor3 = Color3.fromRGB(9,90,255)
UnviewBtn.Text = "Unview"
UnviewBtn.TextColor3 = Color3.new(1,1,1)
UnviewBtn.Font = Enum.Font.SourceSansBold
UnviewBtn.TextSize = 16
UnviewBtn.BorderSizePixel = 0
UnviewBtn.Parent = MainFrame

local ESPInfo = Instance.new("Frame")
ESPInfo.Size = UDim2.new(0.85,0,0,62)
ESPInfo.Position = UDim2.new(0,25,0,112)
ESPInfo.BackgroundColor3 = Color3.fromRGB(50,50,50)
ESPInfo.BorderSizePixel = 0
ESPInfo.Visible = false
ESPInfo.Parent = MainFrame

local ESPSheriff = Instance.new("TextLabel")
ESPSheriff.Size = UDim2.new(1,0,0,28)
ESPSheriff.Position = UDim2.new(0,0,0,4)
ESPSheriff.BackgroundTransparency = 1
ESPSheriff.Text = "Sheriff: ???"
ESPSheriff.TextColor3 = Color3.fromRGB(0,255,170)
ESPSheriff.Font = Enum.Font.SourceSansBold
ESPSheriff.TextSize = 18
ESPSheriff.Parent = ESPInfo

local ESPAssassin = Instance.new("TextLabel")
ESPAssassin.Size = UDim2.new(1,0,0,28)
ESPAssassin.Position = UDim2.new(0,0,0,32)
ESPAssassin.BackgroundTransparency = 1
ESPAssassin.Text = "Assassino: ???"
ESPAssassin.TextColor3 = Color3.fromRGB(255,60,60)
ESPAssassin.Font = Enum.Font.SourceSansBold
ESPAssassin.TextSize = 18
ESPAssassin.Parent = ESPInfo

ESPBtn.MouseButton1Click:Connect(function()
    ESPInfo.Visible = true
end)
UnviewBtn.MouseButton1Click:Connect(function()
    ESPInfo.Visible = false
end)

-- Script Section
local ScriptLabel = Instance.new("TextLabel")
ScriptLabel.Size = UDim2.new(0.7,0,0,28)
ScriptLabel.Position = UDim2.new(0,14,0,185)
ScriptLabel.BackgroundTransparency = 1
ScriptLabel.Text = "Script:"
ScriptLabel.TextColor3 = Color3.fromRGB(255,255,255)
ScriptLabel.Font = Enum.Font.SourceSans
ScriptLabel.TextSize = 18
ScriptLabel.Parent = MainFrame

local AutoFarmBtn = Instance.new("TextButton")
AutoFarmBtn.Size = UDim2.new(0,100,0,26)
AutoFarmBtn.Position = UDim2.new(0,110,0,185)
AutoFarmBtn.BackgroundColor3 = Color3.fromRGB(9,90,255)
AutoFarmBtn.Text = "Auto Farm"
AutoFarmBtn.TextColor3 = Color3.new(1,1,1)
AutoFarmBtn.Font = Enum.Font.SourceSansBold
AutoFarmBtn.TextSize = 16
AutoFarmBtn.BorderSizePixel = 0
AutoFarmBtn.Parent = MainFrame

local UnfarmBtn = Instance.new("TextButton")
UnfarmBtn.Size = UDim2.new(0,70,0,26)
UnfarmBtn.Position = UDim2.new(0,230,0,185)
UnfarmBtn.BackgroundColor3 = Color3.fromRGB(9,90,255)
UnfarmBtn.Text = "Unfarm"
UnfarmBtn.TextColor3 = Color3.new(1,1,1)
UnfarmBtn.Font = Enum.Font.SourceSansBold
UnfarmBtn.TextSize = 16
UnfarmBtn.BorderSizePixel = 0
UnfarmBtn.Parent = MainFrame

local NoclipBtn = Instance.new("TextButton")
NoclipBtn.Size = UDim2.new(0,70,0,26)
NoclipBtn.Position = UDim2.new(0,310,0,185)
NoclipBtn.BackgroundColor3 = Color3.fromRGB(9,90,255)
NoclipBtn.Text = "Noclip"
NoclipBtn.TextColor3 = Color3.new(1,1,1)
NoclipBtn.Font = Enum.Font.SourceSansBold
NoclipBtn.TextSize = 16
NoclipBtn.BorderSizePixel = 0
NoclipBtn.Parent = MainFrame

local CountLabel = Instance.new("TextLabel")
CountLabel.Size = UDim2.new(0,80,0,28)
CountLabel.Position = UDim2.new(0,110,0,220)
CountLabel.BackgroundTransparency = 1
CountLabel.Text = ""
CountLabel.TextColor3 = Color3.fromRGB(9,90,255)
CountLabel.Font = Enum.Font.SourceSansBold
CountLabel.TextSize = 22
CountLabel.Parent = MainFrame

local MensagemLabel = Instance.new("TextLabel")
MensagemLabel.Size = UDim2.new(0.85,0,0,32)
MensagemLabel.Position = UDim2.new(0,25,0,250)
MensagemLabel.BackgroundTransparency = 1
MensagemLabel.Text = ""
MensagemLabel.TextColor3 = Color3.fromRGB(0,255,0)
MensagemLabel.Font = Enum.Font.SourceSansBold
MensagemLabel.TextSize = 16
MensagemLabel.Parent = MainFrame

local runningFarm = false
AutoFarmBtn.MouseButton1Click:Connect(function()
    runningFarm = true
    CountLabel.Text = ""
    MensagemLabel.Text = ""
    for i=1,7 do
        if not runningFarm then break end
        CountLabel.Text = tostring(i)
        wait(0.5)
    end
    if runningFarm then
        MensagemLabel.Text = "Bem vindo by oihi_173 ✨ créditos Copiloto"
    end
end)
UnfarmBtn.MouseButton1Click:Connect(function()
    runningFarm = false
    CountLabel.Text = ""
    MensagemLabel.Text = ""
end)

NoclipBtn.MouseButton1Click:Connect(function()
    MensagemLabel.Text = "Noclip ativado!"
    -- Simulação do noclip (para uso real, inserir script noclip aqui)
end)

-- Rodapé créditos
local Footer = Instance.new("TextLabel")
Footer.Size = UDim2.new(1,0,0,22)
Footer.Position = UDim2.new(0,0,1,-26)
Footer.BackgroundTransparency = 1
Footer.Text = "Créditos: oihi_173 & Copiloto"
Footer.TextColor3 = Color3.fromRGB(170,170,170)
Footer.Font = Enum.Font.SourceSansBold
Footer.TextSize = 14
Footer.Parent = MainFrame

-- Fim do Painel Completo
