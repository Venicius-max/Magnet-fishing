local tool = script.Parent
local magnet = tool:FindFirstChild("Handle")  -- Ímã
local attractionRange = 10  -- Distância em que o ímã pode atrair os objetos
local attractionSpeed = 0.5  -- Velocidade da atração

-- Função para puxar os objetos pescáveis
local function pullItems()
    for _, obj in pairs(workspace:GetChildren()) do
        if obj:IsA("Model") and obj:FindFirstChild("ItemPescável") then
            local base = obj:FindFirstChild("Base")
            if base then
                local distance = (base.Position - magnet.Position).Magnitude
                if distance <= attractionRange then
                    -- Move o objeto em direção ao ímã
                    base.Position = base.Position:Lerp(magnet.Position, attractionSpeed)
                end
            end
        end
    end
end

-- Executa a função continuamente enquanto a ferramenta estiver equipada
tool.Equipped:Connect(function()
    while tool.Parent:IsA("Model") do
        pullItems()
        wait(0.1)  -- Pequeno delay para suavizar o movimento
    end
end)
