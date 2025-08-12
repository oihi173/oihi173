local player = game.Players.LocalPlayer
local backpack = player:WaitForChild("Backpack")

for _, item in pairs(backpack:GetChildren()) do
    if item:IsA("Tool") then
        item.Parent = player.Character
    end
endgame.Replicalocal player = game.Players.LocalPlayer
local character = player.Character
local tool = character:FindFirstChildOfClass("Tool") -- faca ou arma

for _, target in pairs(game.Players:GetPlayers()) do
    if target ~= player and target.Character and target.Character:FindFirstChild("HumanoidRootPart") then
        local targetPart = target.Character.HumanoidRootPart
        tool:Activate() -- Tenta usar a arma (pode variar conforme o jogo)
        -- Aproxima do alvo
        character.HumanoidRootPart.CFrame = targetPart.CFrame
        wait(1)
    end
endtedStorage.DefaultChatSystemChatEvents.SayMessageRequest:FireServer("Olá, estou andando pelo mapa!", "All")
