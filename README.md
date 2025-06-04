--// Painel Nescau Universal - Versão com Mira em NPCs
--// Serviços
local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")
local Workspace = game:GetService("Workspace")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local HttpService = game:GetService("HttpService")

--// Configuração Inicial para Eventos Remotos Dinâmicos
local EventConfig = {
    FireEvent = nil, -- Evento de disparo
    UpdatePosition = nil, -- Evento de movimento
    VerifyHWID = nil, -- Evento de verificação de HWID
}

local function findRemoteEvents()
    for _, obj in pairs(ReplicatedStorage:GetDescendants()) do
        if obj:IsA("RemoteEvent") then
            if not EventConfig.FireEvent and (obj.Name:lower():find("fire") or obj.Name:lower():find("shoot")) then
                EventConfig.FireEvent = obj
            elseif not EventConfig.UpdatePosition and (obj.Name:lower():find("position") or obj.Name:lower():find("move")) then
                EventConfig.UpdatePosition = obj
            elseif not EventConfig.VerifyHWID and (obj.Name:lower():find("hwid") or obj.Name:lower():find("verify")) then
                EventConfig.VerifyHWID = obj
            end
        end
    end
    -- Fallback para nomes genéricos se não encontrados
    EventConfig.FireEvent = EventConfig.FireEvent or ReplicatedStorage:FindFirstChild("FireEvent")
    EventConfig.UpdatePosition = EventConfig.UpdatePosition or ReplicatedStorage:FindFirstChild("UpdatePosition")
    EventConfig.VerifyHWID = EventConfig.VerifyHWID or ReplicatedStorage:FindFirstChild("VerifyHWID")
end

--// 🔒 Obfuscação Avançada
local function generateHash(str)
    local seed = 0
    for i = 1, #str do
        seed = (seed + string.byte(str, i)) * 17 % 999999
    end
    return "x" .. seed .. "_"
end

local function obfuscateCode()
    local hash = generateHash(tostring(math.random(1, 1000000)))
    local vars = {
        aimbot = hash .. "aim",
        esp = hash .. "esp",
        gui = hash .. "interface",
        bypass = hash .. "bypass",
        dummy = hash .. "dummy"
    }
    
    local fakeScript = Instance.new("Script", ReplicatedStorage)
    fakeScript.Source = string.rep("-- Fake code by Painel Nescau\n", math.random(10, 50)) .. "local x = math.random() -- Dummy"
    fakeScript.Name = generateHash("fake" .. tostring(os.clock()))
    
    return vars
end

--// 🔒 Anti-Detection Reforçado
local function antiDetection()
    local protected = setmetatable({}, {
        __index = function(t, k) error("Anti-hook: tentativa de acesso a " .. k) end,
        __newindex = function(t, k, v) error("Anti-hook: tentativa de modificar " .. k) end
    })
    local services = {Players, RunService, Workspace, ReplicatedStorage}
    for _, service in pairs(services) do
        rawset(protected, tostring(service), service)
    end
    
    local sealed = setmetatable({}, {__metatable = "Sealed by Painel Nescau"})
    for k, v in pairs(_G) do
        rawset(sealed, generateHash(k), v)
    end
    
    local dummyContainer = Instance.new("Folder", math.random(1, 2) == 1 and ReplicatedStorage or Workspace)
    dummyContainer.Name = generateHash("container" .. tostring(os.clock()))
    local dummyScript = Instance.new("Script", dummyContainer)
    dummyScript.Source = "-- Painel Nescau Dummy Script"
    
    RunService.Heartbeat:Connect(function()
        if dummyScript.Parent == nil then
            dummyContainer = Instance.new("Folder", math.random(1, 2) == 1 and ReplicatedStorage or Workspace)
            dummyContainer.Name = generateHash("container" .. tostring(os.clock()))
            dummyScript = Instance.new("Script", dummyContainer)
            dummyScript.Source = "-- Painel Nescau Regenerated at " .. os.clock()
        end
    end)
    
    return protected
end

--// GUI Setup
local ScreenGui = Instance.new("ScreenGui", game.CoreGui)
ScreenGui.Name = generateHash("nescau_gui")
local mainFrame = Instance.new("Frame", ScreenGui)
mainFrame.Size = UDim2.new(0, 400, 0, 500)
mainFrame.Position = UDim2.new(0.5, -200, 0.5, -250)
mainFrame.BackgroundColor3 = Color3.fromRGB(50, 0, 100)
mainFrame.BackgroundTransparency = 0.3
mainFrame.Visible = false
mainFrame.Active = true
mainFrame.Name = generateHash("nescau_frame")

-- Título do Painel
local titleLabel = Instance.new("TextLabel", mainFrame)
titleLabel.Size = UDim2.new(1, 0, 0, 30)
titleLabel.Position = UDim2.new(0, 0, 0, -30)
titleLabel.Text = "Painel Nescau"
titleLabel.TextColor3 = Color3.new(1, 1, 1)
titleLabel.BackgroundColor3 = Color3.fromRGB(70, 0, 120)
titleLabel.Font = Enum.Font.SourceSansBold
titleLabel.TextSize = 18
titleLabel.Name = generateHash("nescau_title")

--// Arrastar UI
local dragging, dragInput, dragStart, startPos
mainFrame.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 then
        dragging = true
        dragStart = input.Position
        startPos = mainFrame.Position
    end
end)
mainFrame.InputChanged:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseMovement and dragging then
        local delta = input.Position - dragStart
        mainFrame.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
    end
end)
mainFrame.InputEnded:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 then
        dragging = false
    end
end)

--// Abas da UI
local Tabs = {"Aimbot", "ESP", "Misc", "Configs"}
local tabFrames = {}
local currentTab = nil

for i, tabName in ipairs(Tabs) do
    local tabButton = Instance.new("TextButton", mainFrame)
    tabButton.Size = UDim2.new(0, 100, 0, 30)
    tabButton.Position = UDim2.new(0, (i-1)*100, 0, 0)
    tabButton.Text = tabName
    tabButton.BackgroundColor3 = Color3.fromRGB(70, 0, 120)
    tabButton.TextColor3 = Color3.new(1, 1, 1)
    tabButton.Font = Enum.Font.SourceSansBold
    tabButton.TextSize = 14
    tabButton.Name = generateHash(tabName)
    
    local tabFrame = Instance.new("Frame", mainFrame)
    tabFrame.Size = UDim2.new(1, 0, 1, -30)
    tabFrame.Position = UDim2.new(0, 0, 0, 30)
    tabFrame.BackgroundTransparency = 1
    tabFrame.Visible = false
    tabFrame.Name = generateHash(tabName .. "_frame")
    tabFrames[tabName] = tabFrame
    
    tabButton.MouseButton1Click:Connect(function()
        if currentTab then tabFrames[currentTab].Visible = false end
        tabFrames[tabName].Visible = true
        currentTab = tabName
        logAction("Switched to tab: " .. tabName)
    end)
end

--// Variáveis do Hack
local obfuscated = obfuscateCode()
local flyEnabled = false
local aimbotEnabled = false
local espEnabled = false
local teleportEnabled = false
local teamCheckEnabled = true
local targetNPCsEnabled = false -- Nova opção para mirar em NPCs
local fovRadius = 100
local flySpeed = 50
local silentAim = false
local leadAim = false
local chamsEnabled = false
local skeletonEnabled = false
local showDistance = false
local colorByHealth = false
local streamerMode = false
local aimbotHumanization = true
local desyncEnabled = false
local maxEspRender = 20
local humanizationIntensity = 0.1

--// Visual FOV Circle
local fovCircle = Drawing.new("Circle")
fovCircle.Visible = false
fovCircle.Radius = fovRadius
fovCircle.Color = Color3.fromRGB(150, 100, 255)
fovCircle.Thickness = 2
fovCircle.Filled = false
fovCircle.Name = generateHash("nescau_fov")

--// Função de Log Criptografado
local function logAction(action)
    local success, err = pcall(function()
        local encoded = HttpService:JSONEncode({action = action, time = os.clock(), owner = "Painel Nescau"})
        local logLabel = Instance.new("TextLabel", tabFrames.Configs)
        logLabel.Size = UDim2.new(1, -10, 0, 20)
        logLabel.Position = UDim2.new(0, 5, 0, #tabFrames.Configs:GetChildren() * 25)
        logLabel.Text = "[LOG] " .. action
        logLabel.TextColor3 = Color3.new(1, 1, 1)
        logLabel.BackgroundTransparency = 1
        logLabel.TextSize = 12
        writefile("nescau_hack_log.txt", encoded .. "\n")
    end)
    if not success then
        warn("Painel Nescau: Erro ao registrar log - " .. err)
    end
end

--// Salvar/Carregar Configurações
local function saveConfig()
    local success, err = pcall(function()
        local config = {
            flyEnabled = flyEnabled,
            aimbotEnabled = aimbotEnabled,
            espEnabled = espEnabled,
            teamCheckEnabled = teamCheckEnabled,
            targetNPCsEnabled = targetNPCsEnabled, -- Salvar nova opção
            fovRadius = fovRadius,
            flySpeed = flySpeed,
            silentAim = silentAim,
            leadAim = leadAim,
            chamsEnabled = chamsEnabled,
            skeletonEnabled = skeletonEnabled,
            showDistance = showDistance,
            colorByHealth = colorByHealth,
            aimbotHumanization = aimbotHumanization,
            desyncEnabled = desyncEnabled,
            maxEspRender = maxEspRender,
            humanizationIntensity = humanizationIntensity,
            owner = "Painel Nescau"
        }
        local encoded = HttpService:JSONEncode(config)
        writefile("nescau_hack_config.txt", encoded)
        logAction("Config saved by Painel Nescau")
    end)
    if not success then
        warn("Painel Nescau: Erro ao salvar config - " .. err)
    end
end

local function loadConfig()
    local success, err = pcall(function()
        if isfile("nescau_hack_config.txt") then
            local config = HttpService:JSONDecode(readfile("nescau_hack_config.txt"))
            flyEnabled = config.flyEnabled or false
            aimbotEnabled = config.aimbotEnabled or false
            espEnabled = config.espEnabled or false
            teamCheckEnabled = config.teamCheckEnabled or true
            targetNPCsEnabled = config.targetNPCsEnabled or false -- Carregar nova opção
            fovRadius = config.fovRadius or 100
            flySpeed = config.flySpeed or 50
            silentAim = config.silentAim or false
            leadAim = config.leadAim or false
            chamsEnabled = config.chamsEnabled or false
            skeletonEnabled = config.skeletonEnabled or false
            showDistance = config.showDistance or false
            colorByHealth = config.colorByHealth or false
            aimbotHumanization = config.aimbotHumanization or true
            desyncEnabled = config.desyncEnabled or false
            maxEspRender = config.maxEspRender or 20
            humanizationIntensity = config.humanizationIntensity or 0.1
            logAction("Config loaded by Painel Nescau")
        end
    end)
    if not success then
        warn("Painel Nescau: Erro ao carregar config - " .. err)
    end
end

--// 🧠 Aimbot Furtivo
local Aimbot = {}
function Aimbot:CalculateLead(target, bulletSpeed)
    local targetVelocity = target.Velocity or Vector3.new(0, 0, 0)
    local distance = (target.Position - (LocalPlayer.Character and LocalPlayer.Character.HumanoidRootPart.Position or Vector3.new(0, 0, 0))).Magnitude
    local timeToHit = distance / (bulletSpeed or 300)
    return target.Position + (targetVelocity * timeToHit)
end

function Aimbot:HumanizeAim(targetPos)
    if aimbotHumanization then
        local offset = Vector3.new(
            math.random(-0.5, 0.5),
            math.random(-0.5, 0.5),
            math.random(-0.5, 0.5)
        ) * humanizationIntensity
        return targetPos + offset
    end
    return targetPos
end

function Aimbot:GetClosestTarget()
    local mousePos = UserInputService:GetMouseLocation()
    local closestTarget = nil
    local shortestDistance = math.huge

    -- Suporte a jogadores, NPCs, e objetos interativos
    local targets = {}
    -- Jogadores
    for _, player in pairs(Players:GetPlayers()) do
        if player ~= LocalPlayer and player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
            table.insert(targets, player.Character.HumanoidRootPart)
        end
    end
    -- NPCs (modelos com Humanoid, mas sem Player associado)
    if targetNPCsEnabled then
        for _, obj in pairs(Workspace:GetDescendants()) do
            if obj:IsA("Model") and obj:FindFirstChild("Humanoid") and obj:FindFirstChild("HumanoidRootPart") and not Players:GetPlayerFromCharacter(obj) then
                table.insert(targets, obj.HumanoidRootPart)
            end
        end
    end
    -- Objetos com tag "Targetable"
    for _, obj in pairs(Workspace:GetDescendants()) do
        if obj:IsA("BasePart") and obj:FindFirstChild("Targetable") then
            table.insert(targets, obj)
        end
    end

    for _, target in pairs(targets) do
        local targetPos, onScreen = Workspace.CurrentCamera:WorldToViewportPoint(target.Position)
        if onScreen then
            local screenPos = Vector2.new(targetPos.X, targetPos.Y)
            local distance = (screenPos - Vector2.new(mousePos.X, mousePos.Y)).Magnitude
            if distance < shortestDistance and distance <= fovRadius then
                if not teamCheckEnabled or not target.Parent:FindFirstChild("Team") or target.Parent.Team ~= LocalPlayer.Team then
                    closestTarget = target
                    shortestDistance = distance
                end
            end
        end
    end
    return closestTarget
end

function Aimbot:Run()
    if not aimbotEnabled or not LocalPlayer.Character then
        fovCircle.Visible = streamerMode and false or true
        return
    end
    fovCircle.Visible = streamerMode and false or true
    fovCircle.Position = UserInputService:GetMouseLocation()
    
    local target = Aimbot:GetClosestTarget()
    if target then
        local targetPos = leadAim and Aimbot:CalculateLead(target, 300) or target.Position
        targetPos = Aimbot:HumanizeAim(targetPos)
        
        if math.random() < 0.1 then
            wait(math.random(0.01, 0.05))
        end
        
        if silentAim and EventConfig.FireEvent then
            local ray = Ray.new(Workspace.CurrentCamera.CFrame.Position, (targetPos - Workspace.CurrentCamera.CFrame.Position).Unit * 1000)
            local raycastParams = RaycastParams.new()
            raycastParams.FilterDescendantsInstances = {LocalPlayer.Character}
            raycastParams.FilterType = Enum.RaycastFilterType.Blacklist
            local result = Workspace:Raycast(ray.Origin, ray.Direction, raycastParams)
            if result and result.Instance:IsDescendantOf(target.Parent) then
                pcall(function() EventConfig.FireEvent:FireServer(targetPos) end)
            end
        else
            Workspace.CurrentCamera.CFrame = CFrame.new(Workspace.CurrentCamera.CFrame.Position, targetPos)
        end
    end
end

--// 👁️ ESP Discreto
local espObjects = {}
local function createESPForTarget(target)
    if espObjects[target] then return end
    local box = Drawing.new("Square")
    local skeletonLines = {}
    local healthText = Drawing.new("Text")
    local nameText = Drawing.new("Text")
    local distanceText = Drawing.new("Text")
    
    box.Color = Color3.fromRGB(150, 100, 255)
    box.Thickness = 2
    box.Filled = false
    
    healthText.Color = Color3.new(1, 0, 0)
    healthText.Size = 14
    healthText.Center = true
    healthText.Outline = true
    healthText.Font = 2
    
    nameText.Color = Color3.fromRGB(200, 200, 255)
    nameText.Size = 14
    nameText.Center = true
    nameText.Outline = true
    nameText.Font = 2
    
    distanceText.Color = Color3.new(1, 1, 1)
    distanceText.Size = 14
    distanceText.Center = true
    distanceText.Outline = true
    distanceText.Font = 2
    
    espObjects[target] = {
        Box = box,
        Skeleton = skeletonLines,
        HealthText = healthText,
        NameText = nameText,
        DistanceText = distanceText,
        Highlight = nil
    }
end

local function removeESPForTarget(target)
    if espObjects[target] then
        espObjects[target].Box:Remove()
        espObjects[target].HealthText:Remove()
        espObjects[target].NameText:Remove()
        espObjects[target].DistanceText:Remove()
        for _, line in pairs(espObjects[target].Skeleton) do
            line:Remove()
        end
        if espObjects[target].Highlight then
            espObjects[target].Highlight:Destroy()
        end
        espObjects[target] = nil
    end
end

local function updateESP()
    if not espEnabled or streamerMode then
        for target, _ in pairs(espObjects) do
            removeESPForTarget(target)
        end
        return
    end
    
    local targets = {}
    for _, player in pairs(Players:GetPlayers()) do
        if player ~= LocalPlayer and player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
            table.insert(targets, player.Character)
        end
    end
    for _, obj in pairs(Workspace:GetDescendants()) do
        if obj:IsA("Model") and obj:FindFirstChild("HumanoidRootPart") and (obj:FindFirstChild("Targetable") or obj:FindFirstChild("Humanoid")) then
            table.insert(targets, obj)
        end
    end
    
    -- Limitar renderização para desempenho
    local renderedCount = 0
    table.sort(targets, function(a, b)
        local aDist = (a.HumanoidRootPart.Position - LocalPlayer.Character.HumanoidRootPart.Position).Magnitude
        local bDist = (b.HumanoidRootPart.Position - LocalPlayer.Character.HumanoidRootPart.Position).Magnitude
        return aDist < bDist
    end)
    
    for _, target in ipairs(targets) do
        if renderedCount >= maxEspRender then break end
        local hrp = target:FindFirstChild("HumanoidRootPart")
        local humanoid = target:FindFirstChild("Humanoid")
        local head = target:FindFirstChild("Head")
        
        if hrp and (humanoid or target:FindFirstChild("Targetable")) then
            if not espObjects[target] then
                createESPForTarget(target)
            end
            
            local espData = espObjects[target]
            local cam = Workspace.CurrentCamera
            local hrpPos, onScreenHRP = cam:WorldToViewportPoint(hrp.Position)
            local headPos = head and cam:WorldToViewportPoint(head.Position) or hrpPos
            
            if onScreenHRP then
                renderedCount = renderedCount + 1
                local size = math.clamp(300 / (hrpPos.Z), 20, 100)
                
                espData.Box.Position = Vector2.new(headPos.X - size / 2, headPos.Y - size / 2)
                espData.Box.Size = Vector2.new(size, size)
                espData.Box.Visible = true
                
                espData.NameText.Position = Vector2.new(headPos.X, headPos.Y - size / 2 - 15)
                espData.NameText.Text = target.Name
                espData.NameText.Visible = true
                
                espData.HealthText.Position = Vector2.new(headPos.X, headPos.Y + size / 2 + 5)
                espData.HealthText.Text = humanoid and "HP: " .. math.floor(humanoid.Health) or "Target"
                espData.HealthText.Color = colorByHealth and humanoid and Color3.fromRGB(
                    math.clamp(humanoid.Health * 2.55, 0, 255),
                    math.clamp(255 - humanoid.Health * 2.55, 0, 255),
                    0
                ) or Color3.new(1, 0, 0)
                espData.HealthText.Visible = true
                
                espData.DistanceText.Position = Vector2.new(headPos.X, headPos.Y + size / 2 + 25)
                espData.DistanceText.Text = showDistance and tostring(math.floor((hrp.Position - LocalPlayer.Character.HumanoidRootPart.Position).Magnitude)) .. " studs" or ""
                espData.DistanceText.Visible = showDistance
                
                if chamsEnabled then
                    if not espData.Highlight then
                        espData.Highlight = Instance.new("Highlight")
                        espData.Highlight.Parent = target
                        espData.Highlight.FillColor = colorByHealth and humanoid and Color3.fromRGB(
                            math.clamp(humanoid.Health * 2.55, 0, 255),
                            math.clamp(255 - humanoid.Health * 2.55, 0, 255),
                            0
                        ) or (target:FindFirstChild("Team") and target.Team == LocalPlayer.Team and Color3.new(0, 1, 0) or Color3.new(1, 0, 0))
                        espData.Highlight.OutlineColor = Color3.new(1, 1, 1)
                        espData.Highlight.FillTransparency = math.random(0.3, 0.7)
                    end
                else
                    if espData.Highlight then
                        espData.Highlight:Destroy()
                        espData.Highlight = nil
                    end
                end
                
                if skeletonEnabled and head then
                    for _, line in pairs(espData.Skeleton) do
                        line:Remove()
                    end
                    espData.Skeleton = {}
                    
                    local parts = {"Head", "UpperTorso", "LowerTorso"}
                    local connections = {{"Head", "UpperTorso"}, {"UpperTorso", "LowerTorso"}}
                    
                    for _, conn in pairs(connections) do
                        local part1 = target:FindFirstChild(conn[1])
                        local part2 = target:FindFirstChild(conn[2])
                        if part1 and part2 then
                            local pos1, vis1 = cam:WorldToViewportPoint(part1.Position)
                            local pos2, vis2 = cam:WorldToViewportPoint(part2.Position)
                            if vis1 and vis2 and math.random() < 0.8 then
                                local line = Drawing.new("Line")
                                line.From = Vector2.new(pos1.X, pos1.Y)
                                line.To = Vector2.new(pos2.X, pos2.Y)
                                line.Color = Color3.fromRGB(150, 100, 255)
                                line.Thickness = 1
                                line.Visible = true
                                table.insert(espData.Skeleton, line)
                            end
                        end
                    end
                else
                    for _, line in pairs(espData.Skeleton) do
                        line:Remove()
                    end
                    espData.Skeleton = {}
                end
            else
                espData.Box.Visible = false
                espData.NameText.Visible = false
                espData.HealthText.Visible = false
                espData.DistanceText.Visible = false
                for _, line in pairs(espData.Skeleton) do
                    line.Visible = false
                end
                if espData.Highlight then
                    espData.Highlight:Destroy()
                    espData.Highlight = nil
                end
            end
        else
            removeESPForTarget(target)
        end
    end
end

--// 🧱 Bypass Avançado
local Bypass = {}
function Bypass:FakeInputs()
    local keys = {Enum.KeyCode.W, Enum.KeyCode.A, Enum.KeyCode.S, Enum.KeyCode.D}
    local mouseDelta = Vector2.new(math.random(-5, 5), math.random(-5, 5)) * 0.1
    pcall(function()
        UserInputService.InputBegan:Fire({KeyCode = keys[math.random(1, #keys)], UserInputType = Enum.UserInputType.Keyboard})
        UserInputService.MouseDeltaSensitivity = math.random(0.8, 1.2)
    end)
    logAction("Fake input simulated by Painel Nescau")
end

function Bypass:Desync()
    if not LocalPlayer.Character or not EventConfig.UpdatePosition then return end
    local currentPos = LocalPlayer.Character.HumanoidRootPart.Position
    local fakePos = currentPos + Vector3.new(math.random(-2, 2), 0, math.random(-2, 2))
    pcall(function()
        EventConfig.UpdatePosition:FireServer(fakePos)
        wait(math.random(0.05, 0.15))
        EventConfig.UpdatePosition:FireServer(currentPos)
    end)
    logAction("Desync attempted by Painel Nescau")
end

function Bypass:SpoofHWID()
    if not EventConfig.VerifyHWID then return end
    local fakeHWID = "HWID_" .. HttpService:GenerateGUID(false):sub(1, 16)
    pcall(function() EventConfig.VerifyHWID:FireServer(fakeHWID) end)
    logAction("HWID spoofed by Painel Nescau: " .. fakeHWID)
end

function Bypass:SimulateLatency()
    local delay = math.random(0.01, 0.05)
    wait(delay)
    logAction("Simulated latency of " .. delay .. "s by Painel Nescau")
end

function Bypass:SpoofProperties()
    if LocalPlayer.Character then
        pcall(function()
            LocalPlayer.Character.Humanoid.WalkSpeed = math.random(15, 17)
            LocalPlayer.Character.Humanoid.JumpPower = math.random(49, 51)
        end)
        logAction("Spoofed character properties by Painel Nescau")
    end
end

--// Função para Mover o Player Voando
local function fly()
    if not LocalPlayer.Character then return end
    local humanoidRootPart = LocalPlayer.Character:FindFirstChild("HumanoidRootPart")
    if not humanoidRootPart then return end

    local bodyVelocity = humanoidRootPart:FindFirstChild(generateHash("nescau_velocity"))
    if flyEnabled then
        if not bodyVelocity then
            bodyVelocity = Instance.new("BodyVelocity", humanoidRootPart)
            bodyVelocity.MaxForce = Vector3.new(1e5, 1e5, 1e5)
            bodyVelocity.Velocity = Vector3.new(0, 0, 0)
            bodyVelocity.Name = generateHash("nescau_velocity")
        end

        local moveDir = Vector3.new()
        if UserInputService:IsKeyDown(Enum.KeyCode.W) then moveDir = moveDir + Workspace.CurrentCamera.CFrame.LookVector end
        if UserInputService:IsKeyDown(Enum.KeyCode.S) then moveDir = moveDir - Workspace.CurrentCamera.CFrame.LookVector end
        if UserInputService:IsKeyDown(Enum.KeyCode.A) then moveDir = moveDir - Workspace.CurrentCamera.CFrame.RightVector end
        if UserInputService:IsKeyDown(Enum.KeyCode.D) then moveDir = moveDir + Workspace.CurrentCamera.CFrame.RightVector end
        if UserInputService:IsKeyDown(Enum.KeyCode.Space) then moveDir = moveDir + Vector3.new(0, 1, 0) end
        if UserInputService:IsKeyDown(Enum.KeyCode.LeftControl) then moveDir = moveDir - Vector3.new(0, 1, 0) end

        bodyVelocity.Velocity = moveDir.Unit * flySpeed
        if moveDir.Magnitude == 0 then
            bodyVelocity.Velocity = Vector3.new(0, 0, 0)
        end
    else
        if bodyVelocity then
            bodyVelocity:Destroy()
        end
    end
end

--// Função para Teleportar o Player para o Mouse
local function teleportToMouse()
    local mousePos = UserInputService:GetMouseLocation()
    local camera = Workspace.CurrentCamera
    local ray = camera:ScreenPointToRay(mousePos.X, mousePos.Y)
    local raycastParams = RaycastParams.new()
    raycastParams.FilterDescendantsInstances = {LocalPlayer.Character}
    raycastParams.FilterType = Enum.RaycastFilterType.Blacklist

    local raycastResult = Workspace:Raycast(ray.Origin, ray.Direction * 500, raycastParams)
    if raycastResult and LocalPlayer.Character then
        local humanoidRootPart = LocalPlayer.Character:FindFirstChild("HumanoidRootPart")
        if humanoidRootPart then
            humanoidRootPart.CFrame = CFrame.new(raycastResult.Position + Vector3.new(0, 5, 0))
            logAction("Teleported to mouse position by Painel Nescau")
        end
    end
end

--// 🎥 Modo Streamer Avançado
local function toggleStreamerMode()
    streamerMode = not streamerMode
    mainFrame.BackgroundTransparency = streamerMode and 1 or 0.3
    mainFrame.Visible = not streamerMode
    titleLabel.Visible = not streamerMode
    fovCircle.Visible = streamerMode and false or aimbotEnabled
    for _, espData in pairs(espObjects) do
        espData.Box.Visible = not streamerMode and espEnabled
        espData.NameText.Visible = not streamerMode and espEnabled
        espData.HealthText.Visible = not streamerMode and espEnabled
        espData.DistanceText.Visible = not streamerMode and espEnabled and showDistance
        for _, line in pairs(espData.Skeleton) do
            line.Visible = not streamerMode and espEnabled and skeletonEnabled
        end
        if espData.Highlight then
            espData.Highlight.Enabled = not streamerMode and espEnabled and chamsEnabled
        end
    end
    logAction("Streamer Mode: " .. (streamerMode and "ON" or "OFF") .. " by Painel Nescau")
end

--// Botões da UI com Interação Indireta
local function createToggleButton(parent, text, position, callback)
    local button = Instance.new("TextButton", parent)
    button.Size = UDim2.new(0, 150, 0, 30)
    button.Position = position
    button.Text = text
    button.BackgroundColor3 = Color3.fromRGB(70, 0, 120)
    button.TextColor3 = Color3.new(1, 1, 1)
    button.Font = Enum.Font.SourceSansBold
    button.TextSize = 14
    button.Name = generateHash("nescau_" .. text)
    button.MouseButton1Click:Connect(function()
        if math.random() < 0.95 then
            callback()
        end
    end)
    return button
end

local function createSlider(parent, text, position, min, max, default, callback)
    local frame = Instance.new("Frame", parent)
    frame.Size = UDim2.new(0, 150, 0, 30)
    frame.Position = position
    frame.BackgroundTransparency = 1
    
    local label = Instance.new("TextLabel", frame)
    label.Size = UDim2.new(0.6, 0, 1, 0)
    label.Text = text
    label.TextColor3 = Color3.new(1, 1, 1)
    label.BackgroundTransparency = 1
    label.Font = Enum.Font.SourceSansBold
    label.TextSize = 14
    
    local input = Instance.new("TextBox", frame)
    input.Size = UDim2.new(0.4, 0, 1, 0)
    input.Position = UDim2.new(0.6, 0, 0, 0)
    input.Text = tostring(default)
    input.BackgroundColor3 = Color3.fromRGB(70, 0, 120)
    input.TextColor3 = Color3.new(1, 1, 1)
    input.Font = Enum.Font.SourceSans
    input.TextSize = 14
    
    input.FocusLost:Connect(function()
        local value = tonumber(input.Text)
        if value then
            value = math.clamp(value, min, max)
            input.Text = tostring(value)
            callback(value)
        else
            input.Text = tostring(default)
        end
    end)
end

-- Aimbot Tab
createToggleButton(tabFrames.Aimbot, "Toggle Aimbot", UDim2.new(0, 10, 0, 10), function()
    aimbotEnabled = not aimbotEnabled
    logAction("Aimbot: " .. (aimbotEnabled and "ON" or "OFF") .. " by Painel Nescau")
end)
createToggleButton(tabFrames.Aimbot, "Toggle Silent Aim", UDim2.new(0, 10, 0, 50), function()
    silentAim = not silentAim
    logAction("Silent Aim: " .. (silentAim and "ON" or "OFF") .. " by Painel Nescau")
end)
createToggleButton(tabFrames.Aimbot, "Toggle Lead Aim", UDim2.new(0, 10, 0, 90), function()
    leadAim = not leadAim
    logAction("Lead Aim: " .. (leadAim and "ON" or "OFF") .. " by Painel Nescau")
end)
createToggleButton(tabFrames.Aimbot, "Toggle Team Check", UDim2.new(0, 10, 0, 130), function()
    teamCheckEnabled = not teamCheckEnabled
    logAction("Team Check: " .. (teamCheckEnabled and "ON" or "OFF") .. " by Painel Nescau")
end)
createToggleButton(tabFrames.Aimbot, "Toggle Target NPCs", UDim2.new(0, 10, 0, 170), function() -- Novo botão
    targetNPCsEnabled = not targetNPCsEnabled
    logAction("Target NPCs: " .. (targetNPCsEnabled and "ON" or "OFF") .. " by Painel Nescau")
end)
createToggleButton(tabFrames.Aimbot, "Toggle Humanization", UDim2.new(0, 10, 0, 210), function()
    aimbotHumanization = not aimbotHumanization
    logAction("Humanization: " .. (aimbotHumanization and "ON" or "OFF") .. " by Painel Nescau")
end)
createSlider(tabFrames.Aimbot, "FOV Radius", UDim2.new(0, 10, 0, 250), 50, 500, fovRadius, function(value)
    fovRadius = value
    fovCircle.Radius = value
    logAction("FOV Radius set to " .. value .. " by Painel Nescau")
end)
createSlider(tabFrames.Aimbot, "Humanization Intensity", UDim2.new(0, 10, 0, 290), 0.05, 0.5, humanizationIntensity, function(value)
    humanizationIntensity = value
    logAction("Humanization Intensity set to " .. value .. " by Painel Nescau")
end)

-- ESP Tab
createToggleButton(tabFrames.ESP, "Toggle ESP", UDim2.new(0, 10, 0, 10), function()
    espEnabled = not espEnabled
    logAction("ESP: " .. (espEnabled and "ON" or "OFF") .. " by Painel Nescau")
end)
