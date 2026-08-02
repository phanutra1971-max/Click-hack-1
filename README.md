# Click-hack-1
-- ====================================================================
-- 0. ทำลาย UI เก่าและหยุดลูปเก่าทั้งหมดก่อนรันใหม่
-- ====================================================================
local oldGui = game:GetService("CoreGui"):FindFirstChild("AnimeHubGui")
if oldGui then
    oldGui:SetAttribute("Destroyed", true)
    oldGui:Destroy()
end

-- ====================================================================
-- 1. สร้างโครงสร้าง UI หลัก (Modern Dark UI)
-- ====================================================================
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "AnimeHubGui"
ScreenGui.Parent = game:GetService("CoreGui")
ScreenGui.ResetOnSpawn = false
ScreenGui:SetAttribute("Destroyed", false)

-- หน้าต่างเมนูหลัก
local MainFrame = Instance.new("Frame")
MainFrame.Parent = ScreenGui
MainFrame.Size = UDim2.new(0, 300, 0, 420)
MainFrame.Position = UDim2.new(0.5, -150, 0.5, -210)
MainFrame.BackgroundColor3 = Color3.fromRGB(18, 18, 26)
MainFrame.BorderSizePixel = 0
MainFrame.Active = true
MainFrame.Draggable = true
MainFrame.ClipsDescendants = true

local MainCorner = Instance.new("UICorner")
MainCorner.CornerRadius = UDim.new(0, 12)
MainCorner.Parent = MainFrame

local MainStroke = Instance.new("UIStroke")
MainStroke.Parent = MainFrame
MainStroke.Color = Color3.fromRGB(115, 75, 255)
MainStroke.Thickness = 2
MainStroke.ApplyStrokeMode = Enum.ApplyStrokeMode.Border

-- หัวข้อเมนู
local Title = Instance.new("TextLabel")
Title.Parent = MainFrame
Title.Size = UDim2.new(1, -50, 0, 40)
Title.Position = UDim2.new(0, 15, 0, 0)
Title.BackgroundTransparency = 1
Title.Text = "⚡ ANIME HUB v4.0"
Title.TextColor3 = Color3.fromRGB(255, 255, 255)
Title.TextSize = 18
Title.Font = Enum.Font.GothamBold
Title.TextXAlignment = Enum.TextXAlignment.Left

-- ปุ่มพับเมนู
local ToggleMenuButton = Instance.new("TextButton")
ToggleMenuButton.Parent = ScreenGui
ToggleMenuButton.Size = UDim2.new(0, 28, 0, 28)
ToggleMenuButton.Position = MainFrame.Position + UDim2.new(0, 262, 0, 6)
ToggleMenuButton.BackgroundColor3 = Color3.fromRGB(30, 30, 45)
ToggleMenuButton.Text = "—"
ToggleMenuButton.TextColor3 = Color3.fromRGB(255, 255, 255)
ToggleMenuButton.TextSize = 14
ToggleMenuButton.Font = Enum.Font.GothamBold

local ToggleCorner = Instance.new("UICorner")
ToggleCorner.CornerRadius = UDim.new(0, 8)
ToggleCorner.Parent = ToggleMenuButton

local ToggleStroke = Instance.new("UIStroke")
ToggleStroke.Parent = ToggleMenuButton
ToggleStroke.Color = Color3.fromRGB(115, 75, 255)
ToggleStroke.Thickness = 1.5

local menuOpen = true

-- Scroll Container สำหรับใส่ปุ่มเมนูให้เรียงกันสวยงาม
local ScrollList = Instance.new("ScrollingFrame")
ScrollList.Parent = MainFrame
ScrollList.Size = UDim2.new(1, -20, 1, -50)
ScrollList.Position = UDim2.new(0, 10, 0, 42)
ScrollList.BackgroundTransparency = 1
ScrollList.BorderSizePixel = 0
ScrollList.ScrollBarThickness = 4
ScrollList.ScrollBarImageColor3 = Color3.fromRGB(115, 75, 255)

local UIListLayout = Instance.new("UIListLayout")
UIListLayout.Parent = ScrollList
UIListLayout.SortOrder = Enum.SortOrder.LayoutOrder
UIListLayout.Padding = UDim.new(0, 10)

UIListLayout:GetPropertyChangedSignal("AbsoluteContentSize"):Connect(function()
    ScrollList.CanvasSize = UDim2.new(0, 0, 0, UIListLayout.AbsoluteContentSize.Y + 15)
end)

-- ====================================================================
-- ฟังก์ชั่นสร้าง UI Elements
-- ====================================================================
local layoutOrderIndex = 1

local function CreateStyledButton(text)
    local btn = Instance.new("TextButton")
    btn.Parent = ScrollList
    btn.Size = UDim2.new(1, -8, 0, 42)
    btn.BackgroundColor3 = Color3.fromRGB(25, 20, 32)
    btn.BackgroundTransparency = 0.2
    btn.Text = text
    btn.TextColor3 = Color3.fromRGB(220, 220, 220)
    btn.TextSize = 13
    btn.Font = Enum.Font.GothamSemibold
    btn.AutoButtonColor = false
    btn.LayoutOrder = layoutOrderIndex
    layoutOrderIndex = layoutOrderIndex + 1

    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(0, 8)
    corner.Parent = btn

    local stroke = Instance.new("UIStroke")
    stroke.Name = "BtnStroke"
    stroke.Parent = btn
    stroke.Color = Color3.fromRGB(200, 60, 80)
    stroke.Thickness = 1.5

    btn.MouseEnter:Connect(function() btn.BackgroundTransparency = 0.05 end)
    btn.MouseLeave:Connect(function() btn.BackgroundTransparency = 0.2 end)

    return btn, stroke
end

-- ====================================================================
-- 2. สร้างเมนูฟังก์ชันต่างๆ
-- ====================================================================
local Button1, Stroke1 = CreateStyledButton("Loop Kick : OFF")
local Button2, Stroke2 = CreateStyledButton("Loop Rebirth : OFF")
local Button3, Stroke3 = CreateStyledButton("Loop Reward : OFF")

-- ส่วนของ Reward Input
local InputContainer = Instance.new("Frame")
InputContainer.Parent = ScrollList
InputContainer.Size = UDim2.new(1, -8, 0, 45)
InputContainer.BackgroundTransparency = 1
InputContainer.LayoutOrder = layoutOrderIndex
layoutOrderIndex = layoutOrderIndex + 1

local RewardInput = Instance.new("TextBox")
RewardInput.Parent = InputContainer
RewardInput.Size = UDim2.new(1, 0, 1, 0)
RewardInput.BackgroundColor3 = Color3.fromRGB(15, 12, 22)
RewardInput.Text = "1"
RewardInput.PlaceholderText = "ใส่เลขด่าน (เช่น 1, 2, 5)"
RewardInput.TextColor3 = Color3.fromRGB(115, 200, 255)
RewardInput.PlaceholderColor3 = Color3.fromRGB(120, 120, 140)
RewardInput.TextSize = 13
RewardInput.Font = Enum.Font.GothamBold

local InputCorner = Instance.new("UICorner")
InputCorner.CornerRadius = UDim.new(0, 8)
InputCorner.Parent = RewardInput

local InputStroke = Instance.new("UIStroke")
InputStroke.Parent = RewardInput
InputStroke.Color = Color3.fromRGB(115, 75, 255)
InputStroke.Thickness = 1.5

local Button5, Stroke5 = CreateStyledButton("Loop Hit Wall : OFF")

-- ปุ่มฟังก์ชัน Teleport
local SetPosBtn, SetPosStroke = CreateStyledButton("📍 เซ็ตจุดยืนปัจจุบัน (Set Position)")
SetPosStroke.Color = Color3.fromRGB(115, 75, 255)

local AutoTpBtn, AutoTpStroke = CreateStyledButton("🌀 Auto TP (1s) : OFF")

-- ====================================================================
-- 3. ระบบควบคุมการทำงาน (Logic & Loops)
-- ====================================================================
local loopKickActive = false
local loopRebirthActive = false
local loopRewardActive = false
local loopHitWallActive = false
local autoTpActive = false

local currentRewardZone = 1
local savedCFrame = nil -- ตัวแปรเก็บพิกัดตำแหน่งยืน

local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local KnitServices = ReplicatedStorage:WaitForChild("Packages"):WaitForChild("_Index"):WaitForChild("sleitnick_knit@1.7.0"):WaitForChild("knit"):WaitForChild("Services")

local KickEvent = KnitServices:WaitForChild("KickService"):WaitForChild("RF"):WaitForChild("AddKick")
local RebirthEvent = KnitServices:WaitForChild("RebirthService"):WaitForChild("RF"):WaitForChild("Rebirth")
local RewardEvent = KnitServices:WaitForChild("RewardZoneService"):WaitForChild("RF"):WaitForChild("ClaimReward")
local HitWallEvent = KnitServices:WaitForChild("BreakWallService"):WaitForChild("RF"):WaitForChild("HitWall")

-- กรองช่องตัวเลข Reward
RewardInput:GetPropertyChangedSignal("Text"):Connect(function()
    local cleanText = RewardInput.Text:gsub("%D", "")
    if cleanText ~= RewardInput.Text then RewardInput.Text = cleanText end
    currentRewardZone = tonumber(cleanText) or 1
end)

-- ลูป 1: Kick
local kickArgs = { "Magenta Desk" }
task.spawn(function()
    while true do
        if ScreenGui:GetAttribute("Destroyed") then break end
        if loopKickActive then
            pcall(function() KickEvent:InvokeServer(unpack(kickArgs)) end)
        end
        task.wait()
    end
end)

-- ลูป 2: Rebirth
task.spawn(function()
    while true do
        if ScreenGui:GetAttribute("Destroyed") then break end
        if loopRebirthActive then
            pcall(function() RebirthEvent:InvokeServer() end)
        end
        task.wait(1)
    end
end)

-- ลูป 3: Claim Reward
task.spawn(function()
    while true do
        if ScreenGui:GetAttribute("Destroyed") then break end
        if loopRewardActive then
            pcall(function() RewardEvent:InvokeServer(currentRewardZone) end)
        end
        task.wait(1)
    end
end)

-- ลูป 5: Hit Wall
task.spawn(function()
    while true do
        if ScreenGui:GetAttribute("Destroyed") then break end
        if loopHitWallActive then
            pcall(function() HitWallEvent:InvokeServer() end)
        end
        task.wait()
    end
end)

-- ลูป Auto Teleport ทุก 1 วินาที
task.spawn(function()
    while true do
        if ScreenGui:GetAttribute("Destroyed") then break end
        if autoTpActive and savedCFrame then
            pcall(function()
                local char = LocalPlayer.Character
                if char and char:FindFirstChild("HumanoidRootPart") then
                    char:PivotTo(savedCFrame)
                end
            end)
        end
        task.wait(1)
    end
end)

-- ====================================================================
-- 4. เชื่อมโยงปุ่มกด (Events & Updating UI)
-- ====================================================================
local function UpdateButtonState(btn, stroke, state, onText, offText)
    if state then
        btn.Text = onText
        btn.TextColor3 = Color3.fromRGB(255, 255, 255)
        btn.BackgroundColor3 = Color3.fromRGB(30, 70, 45)
        stroke.Color = Color3.fromRGB(60, 230, 120)
    else
        btn.Text = offText
        btn.TextColor3 = Color3.fromRGB(220, 220, 220)
        btn.BackgroundColor3 = Color3.fromRGB(25, 20, 32)
        stroke.Color = Color3.fromRGB(200, 60, 80)
    end
end

Button1.MouseButton1Click:Connect(function()
    loopKickActive = not loopKickActive
    UpdateButtonState(Button1, Stroke1, loopKickActive, "Loop Kick : ON ⚡", "Loop Kick : OFF")
end)

Button2.MouseButton1Click:Connect(function()
    loopRebirthActive = not loopRebirthActive
    UpdateButtonState(Button2, Stroke2, loopRebirthActive, "Loop Rebirth : ON ⚡", "Loop Rebirth : OFF")
end)

Button3.MouseButton1Click:Connect(function()
    loopRewardActive = not loopRewardActive
    UpdateButtonState(Button3, Stroke3, loopRewardActive, "Loop Reward : ON ⚡", "Loop Reward : OFF")
end)

Button5.MouseButton1Click:Connect(function()
    loopHitWallActive = not loopHitWallActive
    UpdateButtonState(Button5, Stroke5, loopHitWallActive, "Loop Hit Wall : ON ⚡", "Loop Hit Wall : OFF")
end)

-- ปุ่มบันทึกตำแหน่ง
SetPosBtn.MouseButton1Click:Connect(function()
    local char = LocalPlayer.Character
    if char and char:FindFirstChild("HumanoidRootPart") then
        savedCFrame = char.HumanoidRootPart.CFrame
        local pos = savedCFrame.Position
        SetPosBtn.Text = string.format("📍 เซ็ตแล้ว: (%.0f, %.0f, %.0f)", pos.X, pos.Y, pos.Z)
        SetPosBtn.TextColor3 = Color3.fromRGB(115, 200, 255)
    end
end)

-- ปุ่มสวิตช์ Auto TP
AutoTpBtn.MouseButton1Click:Connect(function()
    if not savedCFrame then
        AutoTpBtn.Text = "❌ โปรดเซ็ตจุดก่อนเปิดใช้งาน!"
        task.wait(1.5)
        AutoTpBtn.Text = autoTpActive and "🌀 Auto TP (1s) : ON ⚡" or "🌀 Auto TP (1s) : OFF"
        return
    end
    
    autoTpActive = not autoTpActive
    UpdateButtonState(AutoTpBtn, AutoTpStroke, autoTpActive, "🌀 Auto TP (1s) : ON ⚡", "🌀 Auto TP (1s) : OFF")
end)

-- พับ/กางเมนู
ToggleMenuButton.MouseButton1Click:Connect(function()
    menuOpen = not menuOpen
    if menuOpen then
        MainFrame.Visible = true
        ToggleMenuButton.Text = "—"
        ToggleMenuButton.Position = MainFrame.Position + UDim2.new(0, 262, 0, 6)
    else
        MainFrame.Visible = false
        ToggleMenuButton.Text = "+"
        ToggleMenuButton.Position = UDim2.new(0, 15, 0, 15)
    end
end)

MainFrame:GetPropertyChangedSignal("Position"):Connect(function()
    if menuOpen then
        ToggleMenuButton.Position = MainFrame.Position + UDim2.new(0, 262, 0, 6)
    end
end)
