
local blockRainAmount = 17000  
local rainSpeed = 00000.000.0000.0002 
local spinBlockCount = 90 
local spinRadius = 07 
local spinSpeed = 2  
local blockSize = Vector3.new(2, 2, 2) 
local blockColor = BrickColor.new("Really Red") 


local function createRainBlock()
    local block = Instance.new("Part")
    block.Size = blockSize
    block.BrickColor = blockColor
    block.Anchored = false
    block.CanCollide = true
    block.Position = Vector3.new(
        math.random(-100, 100), 
        math.random(50, 100),  
        math.random(-100, 100) 
    )
    block.Parent = workspace
end


local function spinBlocksAroundPlayer(player)
    if not player or not player.Character or not player.Character:FindFirstChild("HumanoidRootPart") then return end

    local humanoidRootPart = player.Character.HumanoidRootPart
    local spinningBlocks = {}

    
    for i = 1, spinBlockCount do
        local spinBlock = Instance.new("Part")
        spinBlock.Size = blockSize
        spinBlock.BrickColor = BrickColor.new("Bright yellow")
        spinBlock.Anchored = true
        spinBlock.Parent = workspace
        table.insert(spinningBlocks, spinBlock)
    end

    
    local angle = 0
    game:GetService("RunService").Heartbeat:Connect(function()
        if humanoidRootPart then
            angle = angle + spinSpeed * math.rad(1) 
            for i, spinBlock in ipairs(spinningBlocks) do
                local offsetAngle = angle + (math.pi * 2 / #spinningBlocks) * i
                spinBlock.Position = humanoidRootPart.Position + Vector3.new(
                    math.cos(offsetAngle) * spinRadius,
                    
                    math.sin(offsetAngle) * spinRadius
                )
            end
        end
    end)
end


local function selectRandomPlayer()
    local players = game.Players:GetPlayers()
    if #players > 0 then
        return players[math.random(1, #players)]
    end
    return nil
end


for i = 1, blockRainAmount do
    createRainBlock()
    wait(rainSpeed)
end


local randomPlayer = selectRandomPlayer()
if randomPlayer then
    spinBlocksAroundPlayer(randomPlayer)
end
