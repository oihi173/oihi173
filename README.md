local player = game.Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local humanoid = character:WaitForChild("Humanoid")
local rootPart = character:WaitForChild("HumanoidRootPart")

local directions = {
    Vector3.new(1, 0, 0),
    Vector3.new(-1, 0, 0),
    Vector3.new(0, 0, 1),
    Vector3.new(0, 0, -1),
}

local lastDirection = 1

function isPathClear(direction)
    local ray = Ray.new(rootPart.Position, direction * 5)
    local part, pos = workspace:FindPartOnRay(ray, character)
    return part == nil
end

function chooseNextDirection()
    local startIdx = math.random(1, #directions)
    for i = 0, #directions - 1 do
        local idx = ((startIdx + i - 1) % #directions) + 1
        if isPathClear(directions[idx]) then
            return idx
        end
    end
    return startIdx -- Se todos bloqueados, tenta a original
end

while true do
    wait(1)
    local idx = chooseNextDirection()
    lastDirection = idx
    local direction = directions[idx]
    humanoid:MoveTo(rootPart.Position + direction * 10)
    humanoid.MoveToFinished:Wait()
end
