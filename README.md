-- Carregar a Fluent Lib
local Fluent = loadstring(game:HttpGet("https://raw.githubusercontent.com/Stebulous/Fluent/main/source.lua"))()

-- Criar a Janela Principal
local Window = Fluent:CreateWindow({
    Title = "Blox Fruits - Ultimate Script",
    SubTitle = "By SeuNome",
    TabWidth = 160,
    Size = UDim2.fromOffset(500, 500),
    Acrylic = true,
    Theme = "Dark"
})

-- Criar Abas
local FarmTab = Window:AddTab({ Title = "Farm", Icon = "rbxassetid://7072706795" })
local BossTab = Window:AddTab({ Title = "Boss", Icon = "rbxassetid://7072707585" })
local PvPTab = Window:AddTab({ Title = "Bounty", Icon = "rbxassetid://7072707890" })
local RaidTab = Window:AddTab({ Title = "Raids", Icon = "rbxassetid://7072710050" })
local FruitTab = Window:AddTab({ Title = "Frutas", Icon = "rbxassetid://7072710215" })

-- Variáveis globais
_G.AutoFarm = false
_G.AutoQuest = false
_G.AutoBoss = false
_G.AutoBounty = false
_G.AutoRaid = false
_G.AutoFruitSniper = false
_G.AutoChestFarm = false
_G.AutoSeaBeast = false
_G.AutoThirdSeaUnlock = false
_G.AutoHaki = false
_G.AutoDoughRaid = false
_G.FarmSpeed = 1

-- Função para pegar missões automaticamente
function GetQuest()
    local player = game.Players.LocalPlayer
    local questNPC = nil

    -- Procurar NPC de missão
    for _, v in pairs(game:GetService("Workspace").NPCs:GetChildren()) do
        if v:IsA("Model") and v:FindFirstChild("HumanoidRootPart") then
            questNPC = v
            break
        end
    end

    -- Pegar a missão
    if questNPC then
        player.Character.HumanoidRootPart.CFrame = questNPC.HumanoidRootPart.CFrame
        wait(1)
        game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("StartQuest", questNPC.Name, 1)
    end
end

-- Função para comprar frutas automaticamente
function BuyFruit(fruitName)
    local args = {
        [1] = "PurchaseFruit",
        [2] = fruitName
    }
    game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer(unpack(args))
end

-- Função para verificar a loja de frutas e comprar
function CheckShop()
    local fruits = {"Kilo", "Spin", "Chop", "Spring", "Bomb", "Smoke", "Flame", "Ice", "Sand", "Dark", "Light", "Magma", "Rubber", "Barrier", "Ghost", "Quake", "Buddha", "Love", "Spider", "Phoenix", "Portal", "Rumble", "Paw", "Gravity", "Dough", "Shadow", "Venom", "Control", "Spirit", "Dragon", "Leopard"}

    for _, fruit in pairs(fruits) do
        BuyFruit(fruit)
        wait(1)
    end
end

-- Função para Auto Farm
function AutoFarmFunction()
    while _G.AutoFarm do
        local enemy = nil
        for _, v in pairs(game:GetService("Workspace").Enemies:GetChildren()) do
            if v:FindFirstChild("HumanoidRootPart") and v.Humanoid.Health > 0 then
                enemy = v
                break
            end
        end

        if enemy then
            local player = game.Players.LocalPlayer
            local character = player.Character or player.CharacterAdded:Wait()

            -- Movimenta o jogador em direção ao inimigo
            character.HumanoidRootPart.CFrame = enemy.HumanoidRootPart.CFrame * CFrame.new(0, 5, 0)
            -- Ação de atacar (usar habilidade ou apenas atacar com mouse)
            game:GetService("VirtualUser"):CaptureController()
            game:GetService("VirtualUser"):Button1Down(Vector2.new())
        end
        wait(_G.FarmSpeed)
    end
end

-- Ativar Auto Farm
FarmTab:AddToggle("AutoFarm", {
    Title = "Ativar Auto Farm",
    Default = false,
    Callback = function(Value)
        _G.AutoFarm = Value
        if _G.AutoFarm then
            AutoFarmFunction()
        end
    end
})

-- Função para Auto Chest Farm (Coleta todos os baús)
function CollectChests()
    for _, chest in pairs(game:GetService("Workspace").Chest:GetChildren()) do
        if chest:FindFirstChild("HumanoidRootPart") then
            game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = chest.HumanoidRootPart.CFrame
            wait(1)
        end
    end
end

-- Ativar Auto Chest Farm
FarmTab:AddToggle("AutoChestFarm", {
    Title = "Ativar Auto Chest Farm",
    Default = false,
    Callback = function(Value)
        _G.AutoChestFarm = Value
        while _G.AutoChestFarm do
            CollectChests()
            wait(3)
        end
    end
})

-- Função para Auto Sea Beast & Leviathan Farm
function FarmSeaBeast()
    local seaBeast = nil
    for _, v in pairs(game:GetService("Workspace").Enemies:GetChildren()) do
        if v:FindFirstChild("HumanoidRootPart") and v.Name:match("Sea Beast") then
            seaBeast = v
            break
        end
    end

    if seaBeast then
        local player = game.Players.LocalPlayer
        local character = player.Character or player.CharacterAdded:Wait()
        character.HumanoidRootPart.CFrame = seaBeast.HumanoidRootPart.CFrame * CFrame.new(0, 5, 0)
        game:GetService("VirtualUser"):CaptureController()
        game:GetService("VirtualUser"):Button1Down(Vector2.new())
    end
end

-- Ativar Auto Sea Beast
PvPTab:AddToggle("AutoSeaBeast", {
    Title = "Ativar Auto Sea Beast",
    Default = false,
    Callback = function(Value)
        _G.AutoSeaBeast = Value
        while _G.AutoSeaBeast do
            FarmSeaBeast()
            wait(5)
        end
    end
})

-- Função para desbloquear o Third Sea (exemplo simplificado)
function UnlockThirdSea()
    -- Lógica simplificada para desbloquear o Third Sea
    print("Desbloqueando Third Sea...")
end

-- Ativar Auto Third Sea Unlock
RaidTab:AddToggle("AutoThirdSeaUnlock", {
    Title = "Ativar Auto Third Sea Unlock",
    Default = false,
    Callback = function(Value)
        _G.AutoThirdSeaUnlock = Value
        while _G.AutoThirdSeaUnlock do
            UnlockThirdSea()
            wait(10)
        end
    end
})

-- Função para iniciar a Raid da Dough (exemplo simplificado)
function StartDoughRaid()
    -- Lógica simplificada para iniciar a Raid
    print("Iniciando a Raid da Dough...")
end

-- Ativar Auto Dough Raid
RaidTab:AddToggle("AutoDoughRaid", {
    Title = "Ativar Auto Dough Raid",
    Default = false,
    Callback = function(Value)
        _G.AutoDoughRaid = Value
        while _G.AutoDoughRaid do
            StartDoughRaid()
            wait(30)
        end
    end
})

-- Função para pegar missões automaticamente
FarmTab:AddToggle("AutoQuest", {
    Title = "Ativar Auto Quest",
    Default = false,
    Callback = function(Value)
        _G.AutoQuest = Value
        while _G.AutoQuest do
            GetQuest()
            wait(5)
        end
    end
})

-- Mostrar a Janela
Window:SelectTab(1)
