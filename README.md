local player = game.Players.LocalPlayer
local gui = Instance.new("ScreenGui", player:WaitForChild("PlayerGui"))
gui.Name = "CombinedUI"
gui.IgnoreGuiInset = true -- 覆盖整个屏幕

-- ===== 1. 彩色文字（5秒后消失）=====
local textLabel = Instance.new("TextLabel")
textLabel.Text = "REhpoinV1Doors"
textLabel.Size = UDim2.new(0, 400, 0, 100)
textLabel.Position = UDim2.new(0.5, -200, 0.5, -50) -- 居中
textLabel.AnchorPoint = Vector2.new(0.5, 0.5)
textLabel.BackgroundTransparency = 1 -- 透明背景
textLabel.TextScaled = true
textLabel.Font = Enum.Font.SourceSansBold
textLabel.TextColor3 = Color3.new(1, 1, 1)
textLabel.Parent = gui

-- 彩虹颜色变化
local colors = {
    Color3.fromRGB(255, 0, 0),    -- 红
    Color3.fromRGB(255, 127, 0),  -- 橙
    Color3.fromRGB(255, 255, 0),  -- 黄
    Color3.fromRGB(0, 255, 0),    -- 绿
    Color3.fromRGB(0, 0, 255),    -- 蓝
    Color3.fromRGB(75, 0, 130),   -- 靛
    Color3.fromRGB(148, 0, 211)   -- 紫
}

local currentIndex = 1
local transitionTime = 0.5
local timer = 0
local startTime = os.time()

-- ===== 2. 可开关的黑色UI面板（左上角）=====
local blackPanel = Instance.new("Frame")
blackPanel.Name = "BlackPanel"
blackPanel.Size = UDim2.new(0.3, 0, 0.4, 0) -- 宽度30%，高度40%
blackPanel.Position = UDim2.new(0, 10, 0, 10) -- 左上角
blackPanel.BackgroundColor3 = Color3.new(0, 0, 0)
blackPanel.BorderSizePixel = 0
blackPanel.Visible = false -- 默认隐藏
blackPanel.Parent = gui

local toggleButton = Instance.new("TextButton")
toggleButton.Name = "ToggleButton"
toggleButton.Size = UDim2.new(0, 100, 0, 40)
toggleButton.Position = UDim2.new(0, 10, 0, 10)
toggleButton.Text = "打开UI"
toggleButton.BackgroundColor3 = Color3.new(0.2, 0.2, 0.2)
toggleButton.TextColor3 = Color3.new(1, 1, 1)
toggleButton.Parent = gui

local isUIVisible = false
toggleButton.MouseButton1Click:Connect(function()
    isUIVisible = not isUIVisible
    blackPanel.Visible = isUIVisible
    toggleButton.Text = isUIVisible and "隐藏UI" or "打开UI"
end)

-- ===== 3. 主循环（颜色变化 + 5秒后隐藏文字）=====
game:GetService("RunService").RenderStepped:Connect(function(dt)
    -- 颜色变化逻辑
    timer = timer + dt
    if timer >= transitionTime then
        timer = 0
        currentIndex = currentIndex + 1
        if currentIndex > #colors then
            currentIndex = 1
        end
    end
    local nextIndex = currentIndex % #colors + 1
    local alpha = timer / transitionTime
    textLabel.TextColor3 = colors[currentIndex]:Lerp(colors[nextIndex], alpha)

    -- 5秒后隐藏文字
    if os.time() - startTime >= 5 then
        textLabel.Visible = false
    end
end)
