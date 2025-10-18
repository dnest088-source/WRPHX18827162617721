-- Script Broadcast Backdoor Exploit - Inject ke Semua Player
-- Compatible dengan Delta Executor
-- Method: Backdoor Detection + Multiple Injection Methods

local Players = game:GetService("Players")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local Workspace = game:GetService("Workspace")
local TweenService = game:GetService("TweenService")
local LocalPlayer = Players.LocalPlayer

print("=== BACKDOOR WARPAH EXPLOIT ===")
print("Mencari backdoor dan vulnerability...")

-- Daftar nama backdoor yang umum
local BACKDOOR_NAMES = {
    "MainModule", "Loader", "ModuleLoader", "Core", "MainCore",
    "HD Admin", "Padministratior", "Kohl", "PersonalServerAdmin",
    "VanGui", "Epix", "Clone", "Anti", "GamePass", "Model",
    "Script", "LocalScript", "ScreenGui", "RemoteFunction", "RemoteEvent",
    "Handler", "Cmd", "Owner", "Admin", "Ban", "Kick", "Kill"
}

-- Fungsi untuk mencari backdoor
local function FindBackdoors()
    local backdoors = {}
    local searchLocations = {ReplicatedStorage, Workspace, game.Lighting, game.ServerScriptService}
    
    print("[SCAN] Scanning untuk backdoor...")
    
    for _, location in pairs(searchLocations) do
        pcall(function()
            for _, obj in pairs(location:GetDescendants()) do
                -- Cari require() backdoors
                if obj:IsA("ModuleScript") then
                    for _, name in pairs(BACKDOOR_NAMES) do
                        if string.find(string.lower(obj.Name), string.lower(name)) then
                            table.insert(backdoors, {Type = "Module", Object = obj})
                            print("[BACKDOOR FOUND] Module: " .. obj:GetFullName())
                        end
                    end
                end
                
                -- Cari RemoteEvent backdoors
                if obj:IsA("RemoteEvent") then
                    local name = string.lower(obj.Name)
                    if string.find(name, "admin") or string.find(name, "owner") or 
                       string.find(name, "remote") or string.find(name, "command") then
                        table.insert(backdoors, {Type = "Remote", Object = obj})
                        print("[BACKDOOR FOUND] Remote: " .. obj:GetFullName())
                    end
                end
                
                -- Cari RemoteFunction backdoors
                if obj:IsA("RemoteFunction") then
                    table.insert(backdoors, {Type = "Function", Object = obj})
                    print("[BACKDOOR FOUND] Function: " .. obj:GetFullName())
                end
            end
        end)
    end
    
    print("[SCAN] Total backdoor ditemukan: " .. #backdoors)
    return backdoors
end

-- Fungsi untuk exploit ModuleScript backdoor
local function ExploitModuleBackdoor(module, code)
    pcall(function()
        local req = require(module)
        if type(req) == "function" then
            req(code)
            print("[EXPLOIT] Module executed: " .. module.Name)
        elseif type(req) == "table" then
            if req.run then req.run(code) end
            if req.Run then req.Run(code) end
            if req.Execute then req.Execute(code) end
            if req.execute then req.execute(code) end
            if req.Loadstring then req.Loadstring(code) end
            if req.loadstring then req.loadstring(code) end
        end
    end)
end

-- Fungsi untuk exploit RemoteEvent backdoor
local function ExploitRemoteBackdoor(remote, ...)
    local args = {...}
    pcall(function()
        -- Method 1: Direct fire
        remote:FireServer(unpack(args))
        
        -- Method 2: Fire dengan berbagai format
        remote:FireServer("Broadcast", unpack(args))
        remote:FireServer("broadcast", unpack(args))
        remote:FireServer("Execute", unpack(args))
        remote:FireServer("execute", unpack(args))
        remote:FireServer("Command", unpack(args))
        remote:FireServer("command", unpack(args))
        
        print("[EXPLOIT] Remote fired: " .. remote.Name)
    end)
end

-- Fungsi untuk membuat GUI broadcast (akan di-inject via backdoor)
local function GetBroadcastGUICode(message, duration)
    return string.format([[
        local Players = game:GetService("Players")
        local TweenService = game:GetService("TweenService")
        
        for _, player in pairs(Players:GetPlayers()) do
            spawn(function()
                pcall(function()
                    local playerGui = player:WaitForChild("PlayerGui")
                    
                    local oldGui = playerGui:FindFirstChild("BackdoorBroadcast")
                    if oldGui then oldGui:Destroy() end
                    
                    local screenGui = Instance.new("ScreenGui")
                    screenGui.Name = "BackdoorBroadcast"
                    screenGui.ResetOnSpawn = false
                    screenGui.DisplayOrder = 999999
                    screenGui.IgnoreGuiInset = true
                    screenGui.Parent = playerGui
                    
                    local overlay = Instance.new("Frame")
                    overlay.Size = UDim2.new(1, 0, 1, 0)
                    overlay.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
                    overlay.BackgroundTransparency = 0.7
                    overlay.BorderSizePixel = 0
                    overlay.Parent = screenGui
                    
                    local mainFrame = Instance.new("Frame")
                    mainFrame.Size = UDim2.new(0.8, 0, 0.3, 0)
                    mainFrame.Position = UDim2.new(0.1, 0, 0.35, 0)
                    mainFrame.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
                    mainFrame.BorderSizePixel = 3
                    mainFrame.BorderColor3 = Color3.fromRGB(255, 50, 50)
                    mainFrame.Parent = screenGui
                    
                    local corner = Instance.new("UICorner")
                    corner.CornerRadius = UDim.new(0, 15)
                    corner.Parent = mainFrame
                    
                    local title = Instance.new("TextLabel")
                    title.Size = UDim2.new(1, -40, 0, 60)
                    title.Position = UDim2.new(0, 20, 0, 20)
                    title.BackgroundTransparency = 1
                    title.Text = "⚠️ SERVER BROADCAST ⚠️"
                    title.TextSize = 36
                    title.Font = Enum.Font.SourceSansBold
                    title.TextColor3 = Color3.fromRGB(255, 50, 50)
                    title.TextStrokeTransparency = 0.5
                    title.Parent = mainFrame
                    
                    local messageLabel = Instance.new("TextLabel")
                    messageLabel.Size = UDim2.new(1, -60, 1, -100)
                    messageLabel.Position = UDim2.new(0, 30, 0, 85)
                    messageLabel.BackgroundTransparency = 1
                    messageLabel.Text = %q
                    messageLabel.TextSize = 26
                    messageLabel.Font = Enum.Font.SourceSans
                    messageLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
                    messageLabel.TextWrapped = true
                    messageLabel.TextYAlignment = Enum.TextYAlignment.Top
                    messageLabel.Parent = mainFrame
                    
                    mainFrame.Position = UDim2.new(0.1, 0, -0.5, 0)
                    TweenService:Create(mainFrame, TweenInfo.new(0.8, Enum.EasingStyle.Back, Enum.EasingDirection.Out), {
                        Position = UDim2.new(0.1, 0, 0.35, 0)
                    }):Play()
                    
                    spawn(function()
                        while mainFrame.Parent do
                            mainFrame.BorderColor3 = Color3.fromRGB(255, 50, 50)
                            wait(0.5)
                            mainFrame.BorderColor3 = Color3.fromRGB(255, 255, 255)
                            wait(0.5)
                        end
                    end)
                    
                    wait(%d)
                    
                    TweenService:Create(mainFrame, TweenInfo.new(0.6, Enum.EasingStyle.Back, Enum.EasingDirection.In), {
                        Position = UDim2.new(0.1, 0, -0.5, 0)
                    }):Play()
                    
                    wait(0.6)
                    screenGui:Destroy()
                end)
            end)
        end
    ]], message, duration or 10)
end

-- Fungsi utama untuk broadcast via backdoor
local function BackdoorBroadcast(message, duration)
    print("[BROADCAST] Mengeksekusi broadcast via backdoor...")
    
    local backdoors = FindBackdoors()
    local guiCode = GetBroadcastGUICode(message, duration or 10)
    local successCount = 0
    
    -- Exploit semua backdoor yang ditemukan
    for _, backdoor in pairs(backdoors) do
        if backdoor.Type == "Module" then
            ExploitModuleBackdoor(backdoor.Object, guiCode)
            successCount = successCount + 1
        elseif backdoor.Type == "Remote" then
            -- Coba berbagai payload untuk RemoteEvent
            ExploitRemoteBackdoor(backdoor.Object, guiCode)
            ExploitRemoteBackdoor(backdoor.Object, "Broadcast", message)
            ExploitRemoteBackdoor(backdoor.Object, {Type = "Broadcast", Message = message, Code = guiCode})
            successCount = successCount + 1
        elseif backdoor.Type == "Function" then
            pcall(function()
                backdoor.Object:InvokeServer(guiCode)
                backdoor.Object:InvokeServer("Broadcast", message)
            end)
            successCount = successCount + 1
        end
    end
    
    print("[BROADCAST] Exploited " .. successCount .. " backdoors!")
    return successCount > 0
end

-- Fungsi untuk spam semua RemoteEvent dengan GUI code
local function SpamAllRemotes(message, duration)
    print("[SPAM] Firing semua RemoteEvent dengan GUI code...")
    
    local guiCode = GetBroadcastGUICode(message, duration or 10)
    local remotes = {}
    
    -- Collect all RemoteEvents
    for _, obj in pairs(ReplicatedStorage:GetDescendants()) do
        if obj:IsA("RemoteEvent") then
            table.insert(remotes, obj)
        end
    end
    
    for _, obj in pairs(Workspace:GetDescendants()) do
        if obj:IsA("RemoteEvent") then
            table.insert(remotes, obj)
        end
    end
    
    print("[SPAM] Found " .. #remotes .. " RemoteEvents")
    
    -- Fire semuanya
    for _, remote in pairs(remotes) do
        task.spawn(function()
            pcall(function()
                -- Berbagai kombinasi payload
                remote:FireServer(guiCode)
                remote:FireServer("execute", guiCode)
                remote:FireServer("Execute", guiCode)
                remote:FireServer("broadcast", message, guiCode)
                remote:FireServer("Broadcast", message, guiCode)
                remote:FireServer({Code = guiCode, Message = message})
                remote:FireServer({code = guiCode, message = message})
                remote:FireServer("loadstring", guiCode)
                remote:FireServer("Loadstring", guiCode)
                remote:FireServer("run", guiCode)
                remote:FireServer("Run", guiCode)
            end)
        end)
    end
    
    print("[SPAM] All remotes fired!")
end

-- Fungsi untuk mencari dan exploit HD Admin / Perm Admin
local function ExploitHDAdmin(message)
    print("[HD ADMIN] Searching for HD Admin...")
    
    -- Cari HD Admin atau sistem admin lainnya
    local adminRemotes = {}
    
    for _, remote in pairs(ReplicatedStorage:GetDescendants()) do
        if remote:IsA("RemoteEvent") or remote:IsA("RemoteFunction") then
            local name = string.lower(remote.Name)
            if string.find(name, "admin") or string.find(name, "perm") or 
               string.find(name, "owner") or string.find(name, "command") then
                table.insert(adminRemotes, remote)
                print("[FOUND] Admin remote: " .. remote:GetFullName())
            end
        end
    end
    
    -- Try exploit admin commands
    for _, remote in pairs(adminRemotes) do
        if remote:IsA("RemoteEvent") then
            pcall(function()
                remote:FireServer("Broadcast", message)
                remote:FireServer("broadcast", message)
                remote:FireServer("m", message)
                remote:FireServer("h", message)
                remote:FireServer("announce", message)
                remote:FireServer({Command = "broadcast", Args = {message}})
                remote:FireServer({command = "m", args = {message}})
            end)
        elseif remote:IsA("RemoteFunction") then
            pcall(function()
                remote:InvokeServer("Broadcast", message)
                remote:InvokeServer("m", message)
                remote:InvokeServer("h", message)
            end)
        end
    end
    
    print("[HD ADMIN] Exploit attempts completed!")
end

-- Fungsi untuk inject via Chat
local function InjectViaChat(message)
    print("[CHAT] Trying chat injection...")
    
    local chatEvents = ReplicatedStorage:FindFirstChild("DefaultChatSystemChatEvents")
    if chatEvents then
        local sayMessageRequest = chatEvents:FindFirstChild("SayMessageRequest")
        if sayMessageRequest then
            -- Coba kirim dengan special characters untuk bypass filter
            pcall(function()
                sayMessageRequest:FireServer(message, "All")
                sayMessageRequest:FireServer("/broadcast " .. message, "All")
                sayMessageRequest:FireServer("/m " .. message, "All")
                sayMessageRequest:FireServer("/h " .. message, "All")
                sayMessageRequest:FireServer("/announce " .. message, "All")
            end)
            print("[CHAT] Chat injection attempted!")
        end
    end
end

-- Control Panel GUI
local function CreateControlPanel()
    local screenGui = Instance.new("ScreenGui")
    screenGui.Name = "BackdoorControlPanel"
    screenGui.ResetOnSpawn = false
    screenGui.Parent = LocalPlayer.PlayerGui
    
    local frame = Instance.new("Frame")
    frame.Size = UDim2.new(0, 400, 0, 550)
    frame.Position = UDim2.new(0.5, -200, 0.5, -275)
    frame.BackgroundColor3 = Color3.fromRGB(15, 15, 15)
    frame.BorderSizePixel = 3
    frame.BorderColor3 = Color3.fromRGB(255, 0, 0)
    frame.Parent = screenGui
    
    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(0, 12)
    corner.Parent = frame
    
    local title = Instance.new("TextLabel")
    title.Size = UDim2.new(1, -20, 0, 50)
    title.Position = UDim2.new(0, 10, 0, 10)
    title.BackgroundTransparency = 1
    title.Text = "⚠️ BACKDOOR WARPAH EXPLOIT ⚠️"
    title.TextColor3 = Color3.fromRGB(255, 0, 0)
    title.TextSize = 24
    title.Font = Enum.Font.SourceSansBold
    title.TextStrokeTransparency = 0.5
    title.Parent = frame
    
    local subtitle = Instance.new("TextLabel")
    subtitle.Size = UDim2.new(1, -20, 0, 25)
    subtitle.Position = UDim2.new(0, 10, 0, 60)
    subtitle.BackgroundTransparency = 1
    subtitle.Text = "Multi-Method Server Injection"
    subtitle.TextColor3 = Color3.fromRGB(200, 200, 200)
    subtitle.TextSize = 14
    subtitle.Font = Enum.Font.SourceSans
    subtitle.Parent = frame
    
    local textBox = Instance.new("TextBox")
    textBox.Size = UDim2.new(1, -40, 0, 120)
    textBox.Position = UDim2.new(0, 20, 0, 95)
    textBox.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
    textBox.BorderColor3 = Color3.fromRGB(255, 0, 0)
    textBox.BorderSizePixel = 2
    textBox.TextColor3 = Color3.fromRGB(255, 255, 255)
    textBox.TextSize = 16
    textBox.Font = Enum.Font.SourceSans
    textBox.PlaceholderText = "Ketik pesan broadcast disini..."
    textBox.TextWrapped = true
    textBox.MultiLine = true
    textBox.ClearTextOnFocus = false
    textBox.TextXAlignment = Enum.TextXAlignment.Left
    textBox.TextYAlignment = Enum.TextYAlignment.Top
    textBox.Parent = frame
    
    local textCorner = Instance.new("UICorner")
    textCorner.CornerRadius = UDim.new(0, 8)
    textCorner.Parent = textBox
    
    local statusLabel = Instance.new("TextLabel")
    statusLabel.Size = UDim2.new(1, -40, 0, 40)
    statusLabel.Position = UDim2.new(0, 20, 0, 225)
    statusLabel.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
    statusLabel.BorderSizePixel = 0
    statusLabel.Text = "📊 Status: Ready"
    statusLabel.TextColor3 = Color3.fromRGB(0, 255, 0)
    statusLabel.TextSize = 14
    statusLabel.Font = Enum.Font.SourceSansBold
    statusLabel.TextXAlignment = Enum.TextXAlignment.Left
    statusLabel.Parent = frame
    
    local statusCorner = Instance.new("UICorner")
    statusCorner.CornerRadius = UDim.new(0, 8)
    statusCorner.Parent = statusLabel
    
    local function updateStatus(text, color)
        statusLabel.Text = "📊 " .. text
        statusLabel.TextColor3 = color
    end
    
    local function createButton(text, position, color, callback)
        local button = Instance.new("TextButton")
        button.Size = UDim2.new(1, -40, 0, 45)
        button.Position = position
        button.BackgroundColor3 = color
        button.Text = text
        button.TextColor3 = Color3.fromRGB(255, 255, 255)
        button.TextSize = 16
        button.Font = Enum.Font.SourceSansBold
        button.Parent = frame
        
        local btnCorner = Instance.new("UICorner")
        btnCorner.CornerRadius = UDim.new(0, 8)
        btnCorner.Parent = button
        
        button.MouseButton1Click:Connect(callback)
        return button
    end
    
    createButton("🔍 SCAN BACKDOORS", UDim2.new(0, 20, 0, 275), Color3.fromRGB(100, 100, 255), function()
        updateStatus("Scanning...", Color3.fromRGB(255, 255, 0))
        local backdoors = FindBackdoors()
        updateStatus("Found " .. #backdoors .. " backdoors!", Color3.fromRGB(0, 255, 0))
    end)
    
    createButton("💉 BACKDOOR BROADCAST", UDim2.new(0, 20, 0, 330), Color3.fromRGB(255, 100, 0), function()
        local msg = textBox.Text ~= "" and textBox.Text or "🔥 Test Broadcast via Backdoor!"
        updateStatus("Exploiting backdoors...", Color3.fromRGB(255, 255, 0))
        local success = BackdoorBroadcast(msg, 10)
        if success then
            updateStatus("Backdoor exploitation successful!", Color3.fromRGB(0, 255, 0))
            textBox.Text = ""
        else
            updateStatus("No backdoors found!", Color3.fromRGB(255, 0, 0))
        end
    end)
    
    createButton("💥 SPAM ALL REMOTES", UDim2.new(0, 20, 0, 385), Color3.fromRGB(255, 0, 0), function()
        local msg = textBox.Text ~= "" and textBox.Text or "⚠️ Spam Broadcast Test!"
        updateStatus("Spamming all remotes...", Color3.fromRGB(255, 255, 0))
        SpamAllRemotes(msg, 10)
        updateStatus("All remotes fired!", Color3.fromRGB(0, 255, 0))
        textBox.Text = ""
    end)
    
    createButton("🛡️ HD ADMIN EXPLOIT", UDim2.new(0, 20, 0, 440), Color3.fromRGB(150, 0, 255), function()
        local msg = textBox.Text ~= "" and textBox.Text or "📢 HD Admin Broadcast"
        updateStatus("Exploiting HD Admin...", Color3.fromRGB(255, 255, 0))
        ExploitHDAdmin(msg)
        InjectViaChat(msg)
        updateStatus("HD Admin exploit attempted!", Color3.fromRGB(0, 255, 0))
        textBox.Text = ""
    end)
    
    createButton("❌ CLOSE", UDim2.new(0, 20, 0, 495), Color3.fromRGB(80, 80, 80), function()
        screenGui:Destroy()
    end)
    
    -- Draggable
    local dragging, dragInput, dragStart, startPos
    frame.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
            dragging = true
            dragStart = input.Position
            startPos = frame.Position
        end
    end)
    
    frame.InputChanged:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch then
            dragInput = input
        end
    end)
    
    frame.InputEnded:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
            dragging = false
        end
    end)
    
    game:GetService("UserInputService").InputChanged:Connect(function(input)
        if input == dragInput and dragging then
            local delta = input.Position - dragStart
            frame.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
        end
    end)
end

-- Auto execute
print("=== LOADING BACKDOOR EXPLOIT SYSTEM ===")
task.wait(1)
CreateControlPanel()
print("=== SYSTEM READY ===")
print("✅ Control panel loaded!")
print("✅ Scan untuk backdoors dan exploit!")
print("")
print("🔴 WARNING: Script ini menggunakan aggressive exploitation methods!")
print("🔴 Hanya gunakan di game yang memiliki vulnerability!")
