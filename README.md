local player = game.Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local humanoid = character:WaitForChild("Humanoid")
local rootPart = character:WaitForChild("HumanoidRootPart")
local runService = game:GetService("RunService")
local workspace = game:GetService("Workspace")

-- Parâmetros da IA
local moveSpeed = 16
local searchRadius = 30

-- Função para encontrar o item mais próximo
local function findNearestItem()
    local nearestItem = nil
    local shortestDistance = searchRadius
    for _, item in ipairs(workspace:GetChildren()) do
        if item:IsA("Tool") and item.Parent == workspace then
            local distance = (rootPart.Position - item.Position).Magnitude
            if distance < shortestDistance then
                nearestItem = item
                shortestDistance = distance
            end
        end
    end
    return nearestItem
end

-- Função para mover até uma posição
local function moveTo(targetPosition)
    humanoid:MoveTo(targetPosition)
    humanoid.MoveToFinished:Wait()
end

-- Função para explorar uma área aleatória
local function exploreRandomly()
    local randomPosition = rootPart.Position + Vector3.new(
        math.random(-searchRadius, searchRadius),
        0,
        math.random(-searchRadius, searchRadius)
    )
    moveTo(randomPosition)
end

-- Função para pegar item
local function pickUpItem(item)
    if item and (rootPart.Position - item.Position).Magnitude < 5 then
        item.Parent = character
    end
end

-- Loop principal da IA
spawn(function()
    while true do
        local item = findNearestItem()
        if item then
            moveTo(item.Position)
            pickUpItem(item)
        else
            exploreRandomly()
        end
        wait(1)
    end
end)
