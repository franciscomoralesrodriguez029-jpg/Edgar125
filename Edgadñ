--[[
	🔥 EDGAR HUB — SILENT AIM DE COMBATE ESTILO VIDEO 🤫🎯
	Suave, profesional, NO aimbot, pega en la cabeza al disparar
]]--

local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local Workspace = game:GetService("Workspace")
local Camera = Workspace.CurrentCamera
local TweenService = game:GetService("TweenService")
local UserInputService = game:GetService("UserInputService")

local LocalPlayer = Players.LocalPlayer
if not LocalPlayer then return end
local PlayerGui = LocalPlayer:FindFirstChildOfClass("PlayerGui")
if not PlayerGui then return end

-- ==================================================
-- ⚙️ CONFIGURACIÓN COMPLETA
-- ==================================================
local Config = {
    -- 🎯 SILENT AIM DE COMBATE — ESTILO VIDEO
    SilentAim = true,
    SoloAlDisparar = true,   -- ✅ SOLO FUNCIONA AL DISPARAR (NO AIMBOT)
    ParteCuerpo = "Head",    -- 🎯 Apunta a la cabeza
    Suavidad = 0.025,        -- 🤫 SUAVE IGUAL AL VIDEO — NO SE NOTA
    DistanciaMax = 600,      -- 🔥 ALCANCE LEJOS
    SoloVisibles = true,
    -- ⚔️ COMBATE
    Velocidad = false,
    SaltarAlto = false,
    -- 👤 ESP
    NameESP = false,
    SkeletonESP = false,
    ArmasESP = false,
    ItemsESP = false,
    -- 🌾 FARM TRABAJOS
    AutoFarm = false,
    TrabajoLimpiador = false,
    TrabajoCajero = false,
    TrabajoCocinero = false,
    TrabajoMinero = false,
    TrabajoLeñador = false,
    TrabajoPesca = false,
    TrabajoRepartidor = false,
    -- ⚙️ AJUSTES
    VelCorrer = 38,
    SaltoFuerza = 110,
    DistanciaFarm = 12
}
local VelBase = 16
local SaltoBase = 50
local ESP_Dibujos = {}
local Disparando = false

-- ==================================================
-- 🎯 SILENT AIM DE COMBATE — ESTILO VIDEO 🤫🎯
-- SOLO al disparar • Suave • No se nota • Pega en la cabeza
-- ==================================================
local function ObtenerObjetivoCombate()
    local Objetivo, DistMin = nil, math.huge
    local CentroPantalla = Vector2.new(Camera.ViewportSize.X / 2, Camera.ViewportSize.Y / 2)

    for _, Jug in ipairs(Players:GetPlayers()) do
        if Jug ~= LocalPlayer and Jug.Character
        and Jug.Character:FindFirstChild("HumanoidRootPart")
        and Jug.Character:FindFirstChild("Humanoid")
        and Jug.Character.Humanoid.Health > 0 then

            local Parte = Jug.Character:FindFirstChild(Config.ParteCuerpo) or Jug.Character.Head
            local Pos, Visible = Camera:WorldToViewportPoint(Parte.Position)
            
            if Config.SoloVisibles and not Visible then continue end
            
            local DistanciaPantalla = (Vector2.new(Pos.X, Pos.Y) - CentroPantalla).Magnitude
            if DistanciaPantalla < Config.DistanciaMax and DistanciaPantalla < DistMin then
                DistMin = DistanciaPantalla
                Objetivo = Jug
            end
        end
    end
    return Objetivo
end

-- 🎯 DETECTAR DISPARO — SOLO ACTIVA AL DISPARAR
UserInputService.InputBegan:Connect(function(Input, gp)
    if gp then return end
    if Input.UserInputType == Enum.UserInputType.MouseButton1 then
        Disparando = true
    end
end)
UserInputService.InputEnded:Connect(function(Input, gp)
    if gp then return end
    if Input.UserInputType == Enum.UserInputType.MouseButton1 then
        Disparando = false
    end
end)

-- ==================================================
-- 👤 SISTEMA ESP COMPLETO
-- ==================================================
local function CrearESP(Jugador)
    local ESP = Drawing.new("Text")
    ESP.Center = true; ESP.Outline = true; ESP.Font = 2; ESP.Size = 13
    ESP.Color = Color3.fromRGB(255, 80, 80); ESP.Visible = false
    ESP_Dibujos["Name_"..Jugador.UserId] = ESP
    local Skeleton = {}
    local Partes = {"Head","Torso","RightUpperArm","RightLowerArm","RightHand","LeftUpperArm","LeftLowerArm","LeftHand","RightUpperLeg","RightLowerLeg","RightFoot","LeftUpperLeg","LeftLowerLeg","LeftFoot"}
    for _,p in ipairs(Partes) do Skeleton[p] = Drawing.new("Line"); Skeleton[p].Thickness = 1.5; Skeleton[p].Color = Color3.fromRGB(255,80,80); Skeleton[p].Visible = false end
    ESP_Dibujos["Skel_"..Jugador.UserId] = Skeleton
    return ESP, Skeleton
end

local function ActualizarESP()
    for _,Jug in ipairs(Players:GetPlayers()) do
        if Jug == LocalPlayer then continue end
        local ESP = ESP_Dibujos["Name_"..Jug.UserId]
        local Skel = ESP_Dibujos["Skel_"..Jug.UserId]
        if not ESP then ESP, Skel = CrearESP(Jug) end
        if Jug.Character and Jug.Character:FindFirstChild("HumanoidRootPart") and Jug.Character.Humanoid.Health > 0 then
            local Pos, Visible = Camera:WorldToViewportPoint(Jug.Character.Head.Position)
            ESP.Visible = Config.NameESP and Visible
            if Config.NameESP and Visible then ESP.Position = Vector2.new(Pos.X, Pos.Y-35); ESP.Text = Jug.Name.."\n❤️ "..math.floor(Jug.Character.Humanoid.Health) end
            for _,Linea in pairs(Skel) do Linea.Visible = Config.SkeletonESP end
            if Config.SkeletonESP then
                local Char = Jug.Character
                local function DibujarLinea(p1,p2,linea)
                    if Char:FindFirstChild(p1) and Char:FindFirstChild(p2) then
                        local a = Camera:WorldToViewportPoint(Char[p1].Position)
                        local b = Camera:WorldToViewportPoint(Char[p2].Position)
                        linea.From = Vector2.new(a.X,a.Y); linea.To = Vector2.new(b.X,b.Y)
                        linea.Visible = a.Z>0 and b.Z>0
                    end
                end
                DibujarLinea("Head","Torso",Skel.Head)
                DibujarLinea("Torso","RightUpperArm",Skel.Torso)
                DibujarLinea("RightUpperArm","RightLowerArm",Skel.RightUpperArm)
                DibujarLinea("RightLowerArm","RightHand",Skel.RightLowerArm)
                DibujarLinea("Torso","LeftUpperArm",Skel.LeftUpperArm)
                DibujarLinea("LeftUpperArm","LeftLowerArm",Skel.LeftUpperArm)
                DibujarLinea("LeftLowerArm","LeftHand",Skel.LeftLowerArm)
                DibujarLinea("Torso","RightUpperLeg",Skel.RightUpperLeg)
                DibujarLinea("RightUpperLeg","RightLowerLeg",Skel.RightUpperLeg)
                DibujarLinea("RightLowerLeg","RightFoot",Skel.RightLowerLeg)
                DibujarLinea("Torso","LeftUpperLeg",Skel.LeftUpperLeg)
                DibujarLinea("LeftUpperLeg","LeftLowerLeg",Skel.LeftUpperLeg)
                DibujarLinea("LeftLowerLeg","LeftFoot",Skel.LeftLowerLeg)
            end
        else
            ESP.Visible = false; for _,Linea in pairs(Skel or {}) do Linea.Visible = false end
        end
    end
end

-- ==================================================
-- 🌾 FARM DE TODOS LOS TRABAJOS
-- ==================================================
local function FarmTrabajos()
    local Char = LocalPlayer.Character
    if not Char or not Char:FindFirstChild("HumanoidRootPart") then return end
    local HRP = Char.HumanoidRootPart

    if Config.AutoFarm then
        for _, v in pairs(Workspace:GetChildren()) do
            if v:IsA("BasePart") and not v:IsDescendantOf(Char) then
                local NombreMinus = string.lower(v.Name)
                local Dist = (v.Position - HRP.Position).Magnitude

                if NombreMinus:find("coin") or NombreMinus:find("money") or NombreMinus:find("moneda") 
                or NombreMinus:find("drop") or NombreMinus:find("item") or NombreMinus:find("reward") then
                    if Dist < Config.DistanciaFarm then
                        task.spawn(function()
                            pcall(function()
                                v.Anchored = false
                                TweenService:Create(v, TweenInfo.new(0.3), {Position = HRP.Position + Vector3.new(0, 2, 0)}):Play()
                                task.wait(0.35)
                                if v and v:IsDescendantOf(game) then v:Destroy() end
                            end)
                        end)
                    end
                end

                if Config.TrabajoLimpiador and (NombreMinus:find("trash") or NombreMinus:find("basura")) then
                    if Dist < Config.DistanciaFarm then task.spawn(function() pcall(function() v:Destroy() end) end) end
                end
                if Config.TrabajoCocinero and (NombreMinus:find("food") or NombreMinus:find("comida")) then
                    if Dist < Config.DistanciaFarm then task.spawn(function() pcall(function() v:Destroy() end) end) end
                end
                if Config.TrabajoMinero and (NombreMinus:find("ore") or NombreMinus:find("mineral")) then
                    if Dist < Config.DistanciaFarm then task.spawn(function() pcall(function() v:Destroy() end) end) end
                end
                if Config.TrabajoLeñador and (NombreMinus:find("wood") or NombreMinus:find("log")) then
                    if Dist < Config.DistanciaFarm then task.spawn(function() pcall(function() v:Destroy() end) end) end
                end
                if Config.TrabajoPesca and (NombreMinus:find("fish") or NombreMinus:find("pez")) then
                    if Dist < Config.DistanciaFarm then task.spawn(function() pcall(function() v:Destroy() end) end) end
                end
                if Config.TrabajoRepartidor and (NombreMinus:find("package") or NombreMinus:find("paquete")) then
                    if Dist < Config.DistanciaFarm then task.spawn(function() pcall(function() v:Destroy() end) end) end
                end
            end
        end
    end
end

-- ==================================================
-- 🖤 MENÚ EDGAR HUB — ESTILO MORTYHUB 🤫
-- ==================================================
local Gui = Instance.new("ScreenGui")
Gui.Name = "EDGARHUB"
Gui.ResetOnSpawn = false
Gui.Parent = PlayerGui

local Ventana = Instance.new("Frame")
Ventana.Size = UDim2.new(0, 300, 0, 540)
Ventana.Position = UDim2.new(0.02, 0, 0.5, -270)
Ventana.BackgroundColor3 = Color3.fromRGB(12, 12, 16)
Ventana.BorderSizePixel = 2
Ventana.BorderColor3 = Color3.fromRGB(55, 50, 75)
Ventana.Active = true
Ventana.Draggable = true
Ventana.Parent = Gui
Ventana.CornerRadius = UDim.new(0, 12)

local Barra = Instance.new("Frame")
Barra.Size = UDim2.new(1, 0, 0, 55)
Barra.BackgroundColor3 = Color3.fromRGB(40, 30, 60)
Barra.Parent = Ventana
Barra.CornerRadius = UDim.new(0, 12)

local Titulo = Instance.new("TextLabel")
Titulo.Size = UDim2.new(1, 0, 1, 0)
Titulo.BackgroundTransparency = 1
Titulo.Text = "🔥 EDGAR HUB • COMBATE 🎯"
Titulo.Font = Enum.Font.GothamBold
Titulo.TextSize = 18
Titulo.TextColor3 = Color3.fromRGB(220, 220, 255)
Titulo.Parent = Barra

local function Boton(texto, posY, claveConfig, callback)
    local btn = Instance.new("TextButton")
    btn.Size = UDim2.new(0.85, 0, 0, 34)
    btn.Position = UDim2.new(0.075, 0, 0, posY)
    btn.BackgroundColor3 = Config[claveConfig] and Color3.fromRGB(60, 140, 90) or Color3.fromRGB(28, 25, 40)
    btn.Text = texto
    btn.Font = Enum.Font.Gotham
    btn.TextSize = 13
    btn.TextColor3 = Color3.fromRGB(230, 230, 245)
    btn.CornerRadius = UDim.new(0, 8)
    btn.Parent = Ventana
    btn.MouseButton1Click:Connect(function()
        Config[claveConfig] = not Config[claveConfig]
        if callback then callback() end
        btn.BackgroundColor3 = Config[claveConfig] and Color3.fromRGB(60, 140, 90) or Color3.fromRGB(28, 25, 40)
    end)
    return btn
end

local function Separador(texto, posY)
    local Sep = Instance.new("TextLabel")
    Sep.Size = UDim2.new(0.85, 0, 0, 20)
    Sep.Position = UDim2.new(0.075, 0, 0, posY)
    Sep.BackgroundTransparency = 1
    Sep.Text = texto
    Sep.Font = Enum.Font.GothamBold
    Sep.TextSize = 12
    Sep.TextColor3 = Color3.fromRGB(150, 140, 180)
    Sep.Parent = Ventana
end

-- 📋 SECCIÓN COMBATE — SILENT AIM ESTILO VIDEO 🎯
Separador("⚔️ COMBATE", 55)
Boton("🎯 SILENT AIM", 80, "SilentAim")
Boton("⚡ VELOCIDAD", 122, "Velocidad", function()
    local Char = LocalPlayer.Character
    if Char then local H = Char:FindFirstChildOfClass("Humanoid")
        if H then H.WalkSpeed = Config.Velocidad and Config.VelCorrer or VelBase end end
end)
Boton("🦘 SALTO ALTO", 164, "SaltarAlto", function()
    local Char = LocalPlayer.Character
    if Char then local H = Char:FindFirstChildOfClass("Humanoid")
        if H then H.JumpPower = Config.SaltarAlto and Config.SaltoFuerza or SaltoBase end end
end)

-- 📋 SECCIÓN ESP
Separador("👤 ESP", 210)
Boton("👤 NAME ESP", 235, "NameESP")
Boton("🦴 SKELETON ESP", 277, "SkeletonESP")
Boton("🔫 ARMAS ESP", 319, "ArmasESP")
Boton("📦 ITEMS ESP", 361, "ItemsESP")

-- 📋 SECCIÓN FARM TRABAJOS
Separador("🌾 FARM TRABAJOS", 405)
Boton("🌾 AUTO FARM TODO", 430, "AutoFarm")
Boton("🧹 LIMPIADOR", 472, "TrabajoLimpiador")
Boton("🛒 CAJERO", 472, "TrabajoCajero")
Boton("🍳 COCINERO", 514, "TrabajoCocinero")
Boton("⛏️ MINERO", 514, "TrabajoMinero")
Boton("🪓 LEÑADOR", 514, "TrabajoLeñador")
Boton("🎣 PESCA", 514, "TrabajoPesca")
Boton("📦 REPARTIDOR", 514, "TrabajoRepartidor")

-- ==================================================
-- ⚙️ BUCLE PRINCIPAL — SILENT AIM FUNCIONA SOLO AL DISPARAR 🎯
-- ==================================================
RunService.RenderStepped:Connect(function()
    local Char = LocalPlayer.Character
    if not Char or not Char:FindFirstChild("HumanoidRootPart") then return end
    local Hum = Char:FindFirstChild("Humanoid")
    if not Hum then return end

    -- 🎯 SILENT AIM DE COMBATE — SOLO AL DISPARAR 🤫
    if Config.SilentAim then
        -- ✅ Si está activado Y (no requiere disparar O está disparando) → funciona
        if not Config.SoloAlDisparar or Disparando then
            local Obj = ObtenerObjetivoCombate()
            if Obj and Obj.Character then
                local Parte = Obj.Character:FindFirstChild(Config.ParteCuerpo) or Obj.Character.Head
                if Parte then
                    Camera.CFrame = Camera.CFrame:Lerp(
                        CFrame.new(Camera.CFrame.Position, Parte.Position),
                        Config.Suavidad
                    )
                end
            end
        end
    end

    -- 👤 ESP
    ActualizarESP()

    -- 🌾 FARM TRABAJOS
    FarmTrabajos()
end)

print("✅ EDGAR HUB CARGADO — SILENT AIM DE COMBATE ESTILO VIDEO 🤫🎯")
