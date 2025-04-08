--== AUTO SERVER HOP - CÓ NÚT ẨN/HIỆN MENU ==--

local Players = game:GetService("Players")
local TeleportService = game:GetService("TeleportService")
local HttpService = game:GetService("HttpService")
local RunService = game:GetService("RunService")
local Lighting = game:GetService("Lighting")
local Terrain = workspace.Terrain
local LP = Players.LocalPlayer

local autoHopEnabled = true
local delayTime = 2400 -- 40 phút
local hopCount = 0
local JobIdList = {}

-- GIẢM LAG
settings().Rendering.QualityLevel = Enum.QualityLevel.Level01
for _, v in pairs(game:GetDescendants()) do
    if v:IsA("ParticleEmitter") or v:IsA("Trail") or v:IsA("Smoke") or v:IsA("Fire") then
        v.Enabled = false
    elseif v:IsA("Decal") or v:IsA("Texture") then
        v.Transparency = 1
    elseif v:IsA("PointLight") or v:IsA("SurfaceLight") or v:IsA("SpotLight") then
        v.Enabled = false
    elseif v:IsA("MeshPart") or v:IsA("Part") then
        v.Material = Enum.Material.SmoothPlastic
        v.Reflectance = 0
    end
end
Lighting.GlobalShadows = false
Lighting.FogEnd = 1e10
Lighting.Brightness = 0
Lighting.ClockTime = 12
Lighting.Technology = Enum.Technology.Compatibility
Terrain.WaterWaveSize, Terrain.WaterWaveSpeed, Terrain.WaterReflectance, Terrain.WaterTransparency = 0, 0, 0, 1

-- UI
local gui = Instance.new("ScreenGui", LP:WaitForChild("PlayerGui"))
gui.Name = "AutoHopGui"
gui.ResetOnSpawn = false

-- NÚT ẨN/HIỆN MENU
local toggleMenu = Instance.new("TextButton", gui)
toggleMenu.Size = UDim2.new(0, 80, 0, 30)
toggleMenu.Position = UDim2.new(0, 10, 0, 10)
toggleMenu.BackgroundColor3 = Color3.fromRGB(30,30,30)
toggleMenu.TextColor3 = Color3.new(1,1,1)
toggleMenu.Text = "Ẩn Menu"
toggleMenu.Font = Enum.Font.SourceSansBold
toggleMenu.TextSize = 14

-- MENU CHÍNH
local frame = Instance.new("Frame", gui)
frame.Size = UDim2.new(0, 230, 0, 140)
frame.Position = UDim2.new(1, -240, 0, 10)
frame.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
frame.BorderSizePixel = 0
frame.Active = true
frame.Draggable = true

local title = Instance.new("TextLabel", frame)
title.Size = UDim2.new(1, 0, 0, 24)
title.Text = "AUTO SERVER HOP"
title.Font = Enum.Font.GothamBold
title.TextColor3 = Color3.new(1,1,1)
title.TextSize = 14
title.BackgroundTransparency = 1

local function rainbow(t)
    return Color3.fromRGB(math.sin(t)*127+128, math.sin(t+2)*127+128, math.sin(t+4)*127+128)
end

local info = Instance.new("TextLabel", frame)
info.Position = UDim2.new(0, 0, 0, 25)
info.Size = UDim2.new(1, 0, 0, 20)
info.BackgroundTransparency = 1
info.Font = Enum.Font.SourceSansBold
info.TextSize = 14
info.TextColor3 = Color3.new(1, 1, 1)
info.Text = "Đợi..."

local toggle = Instance.new("TextButton", frame)
toggle.Position = UDim2.new(0, 10, 0, 50)
toggle.Size = UDim2.new(1, -20, 0, 24)
toggle.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
toggle.Text = "TẮT AUTO HOP"
toggle.Font = Enum.Font.SourceSans
toggle.TextSize = 13
toggle.TextColor3 = Color3.new(1, 1, 1)
toggle.MouseButton1Click:Connect(function()
    autoHopEnabled = not autoHopEnabled
    toggle.Text = autoHopEnabled and "TẮT AUTO HOP" or "BẬT AUTO HOP"
end)

local hopNow = Instance.new("TextButton", frame)
hopNow.Position = UDim2.new(0, 10, 0, 80)
hopNow.Size = UDim2.new(1, -20, 0, 24)
hopNow.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
hopNow.Text = "CHUYỂN SERVER NGAY"
hopNow.Font = Enum.Font.SourceSansBold
hopNow.TextSize = 13
hopNow.TextColor3 = Color3.new(1, 1, 1)
hopNow.MouseButton1Click:Connect(function()
    if autoHopEnabled then
        ServerHop()
    end
end)

-- Nút ẩn/hiện hoạt động
local menuVisible = true
toggleMenu.MouseButton1Click:Connect(function()
    menuVisible = not menuVisible
    frame.Visible = menuVisible
    toggleMenu.Text = menuVisible and "Ẩn Menu" or "Hiện Menu"
end)

-- Server Hop
function ServerHop()
    local code = [[
        loadstring(game:HttpGet("https://pastebin.com/raw/DPnX0z2S"))()
    ]]
    queue_on_teleport(code)

    local servers = HttpService:JSONDecode(game:HttpGet("https://games.roblox.com/v1/games/"..game.PlaceId.."/servers/Public?sortOrder=Asc&limit=100"))
    for _, s in pairs(servers.data) do
        if s.playing < s.maxPlayers and s.id ~= game.JobId and not table.find(JobIdList, s.id) then
            table.insert(JobIdList, s.id)
            hopCount += 1
            TeleportService:TeleportToPlaceInstance(game.PlaceId, s.id, LP)
            break
        end
    end
end

-- FPS Counter
local fps, frames, last = 60, 0, tick()
RunService.RenderStepped:Connect(function()
    frames += 1
    if tick() - last >= 1 then
        fps = frames
        frames = 0
        last = tick()
    end
end)

-- Countdown Loop
task.spawn(function()
    while true do
        for t = delayTime, 0, -1 do
            local min, sec = math.floor(t/60), t%60
            info.TextColor3 = rainbow(tick())
            info.Text = string.format("FPS: %d | Còn: %02d:%02d | Lần: %d", fps, min, sec, hopCount)
            task.wait(1)
        end
        if autoHopEnabled then ServerHop() end
    end
end)
