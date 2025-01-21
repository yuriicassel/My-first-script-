
local blockRainAmount = 17000 -- Quantidade de blocos que vão cair
local rainSpeed = 00000.000.0000.0002 -- Intervalo entre a criação de blocos
local spinBlockCount = 90 -- Número de blocos que giram em torno de um jogador
local spinRadius = 07 -- Distância do bloco em relação ao jogador
local spinSpeed = 2 -- Velocidade da rotação
local blockSize = Vector3.new(2, 2, 2) -- Tamanho dos blocos
local blockColor = BrickColor.new("Really Red") -- Cor dos blocos


local function createRainBlock()
    local block = Instance.new("Part")
    block.Size = blockSize
    block.BrickColor = blockColor
    block.Anchored = false
    block.CanCollide = true
    block.Position = Vector3.new(
        math.random(-100, 100), -- Posição aleatória no eixo X
        math.random(50, 100),  -- Altura inicial
        math.random(-100, 100) -- Posição aleatória no eixo Z
    )
    block.Parent = workspace
end


local function spinBlocksAroundPlayer(player)
    if not player or not player.Character or not player.Character:FindFirstChild("HumanoidRootPart") then return end

    local humanoidRootPart = player.Character.HumanoidRootPart
    local spinningBlocks = {}

    -- Criar os blocos que irão girar
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
            angle = angle + spinSpeed * math.rad(1) -- Aumentar o ângulo gradualmente
            for i, spinBlock in ipairs(spinningBlocks) do
                local offsetAngle = angle + (math.pi * 2 / #spinningBlocks) * i
                spinBlock.Position = humanoidRootPart.Position + Vector3.new(
                    math.cos(offsetAngle) * spinRadius,
                    3, -- Altura em relação ao jogador
                    math.sin(offsetAngle) * spinRadius
                )
            end
        end
    end)
