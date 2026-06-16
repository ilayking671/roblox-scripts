--!strict
-- Services
local Players    = game:GetService("Players")
local UIS        = game:GetService("UserInputService")
local TweenSvc   = game:GetService("TweenService")

-- Player / GUI setup
local player    = Players.LocalPlayer
local playerGui = player:WaitForChild("PlayerGui")

local function cleanupExistingGui()
    for _, v in ipairs(playerGui:GetChildren()) do
        if v:IsA("ScreenGui") and v.Name == "CodeExecutor" then v:Destroy() end
    end
end
cleanupExistingGui()

local gui = Instance.new("ScreenGui")
gui.Name                  = "CodeExecutor"
gui.ResetOnSpawn          = false
gui.ZIndexBehavior        = Enum.ZIndexBehavior.Sibling
gui.IgnoreGuiInset        = true   -- covers full screen including topbar
gui.Parent                = playerGui

-- ─────────────────────────────────────────────────────────────
-- LOADING SCREEN  (shown before main GUI builds)
-- ─────────────────────────────────────────────────────────────
local loadScreen = Instance.new("Frame")
loadScreen.Name             = "LoadScreen"
loadScreen.Size             = UDim2.fromScale(1, 1)
loadScreen.Position         = UDim2.fromScale(0, 0)
loadScreen.BackgroundColor3 = Color3.fromRGB(8, 8, 8)
loadScreen.BorderSizePixel  = 0
loadScreen.ZIndex           = 200
loadScreen.Parent           = gui

-- Subtle vignette gradient
local vignette = Instance.new("UIGradient")
vignette.Color = ColorSequence.new({
    ColorSequenceKeypoint.new(0, Color3.fromRGB(0,0,0)),
    ColorSequenceKeypoint.new(0.5, Color3.fromRGB(14,14,14)),
    ColorSequenceKeypoint.new(1, Color3.fromRGB(0,0,0)),
})
vignette.Rotation = 45
vignette.Parent = loadScreen

-- Fetch game info from MarketplaceService
local gameName    = "Unknown Game"
local gameCreator = "Unknown"
local ok, info = pcall(function()
    return game:GetService("MarketplaceService"):GetProductInfo(game.PlaceId)
end)
if ok and info then
    gameName    = info.Name or gameName
    gameCreator = info.Creator and info.Creator.Name or gameCreator
end

-- Center container
local loadCenter = Instance.new("Frame")
loadCenter.Size             = UDim2.fromOffset(340, 240)
loadCenter.Position         = UDim2.fromScale(0.5, 0.5)
loadCenter.AnchorPoint      = Vector2.new(0.5, 0.5)
loadCenter.BackgroundTransparency = 1
loadCenter.ZIndex           = 201
loadCenter.Parent           = loadScreen

-- <> logo
local loadLogo = Instance.new("TextLabel")
loadLogo.Size               = UDim2.fromOffset(320, 60)
loadLogo.BackgroundTransparency = 1
loadLogo.Text               = "<>"
loadLogo.TextColor3         = Color3.fromRGB(60, 220, 120)
loadLogo.TextSize           = 42
loadLogo.Font               = Enum.Font.Code
loadLogo.TextXAlignment     = Enum.TextXAlignment.Center
loadLogo.ZIndex             = 202
loadLogo.Parent             = loadCenter

-- App name
local loadName = Instance.new("TextLabel")
loadName.Size               = UDim2.fromOffset(320, 32)
loadName.Position           = UDim2.fromOffset(0, 58)
loadName.BackgroundTransparency = 1
loadName.Text               = "Kernel"
loadName.TextColor3         = Color3.fromRGB(220, 220, 220)
loadName.TextSize           = 22
loadName.Font               = Enum.Font.GothamBold
loadName.TextXAlignment     = Enum.TextXAlignment.Center
loadName.ZIndex             = 202
loadName.Parent             = loadCenter

-- Version tag
local loadVersion = Instance.new("TextLabel")
loadVersion.Size            = UDim2.fromOffset(320, 20)
loadVersion.Position        = UDim2.fromOffset(0, 90)
loadVersion.BackgroundTransparency = 1
loadVersion.Text            = "v4  —  Executor"
loadVersion.TextColor3      = Color3.fromRGB(80, 80, 80)
loadVersion.TextSize        = 12
loadVersion.Font            = Enum.Font.Gotham
loadVersion.TextXAlignment  = Enum.TextXAlignment.Center
loadVersion.ZIndex          = 202
loadVersion.Parent          = loadCenter

-- Divider line
local loadDivider = Instance.new("Frame")
loadDivider.Size            = UDim2.fromOffset(260, 1)
loadDivider.Position        = UDim2.fromOffset(40, 116)
loadDivider.BackgroundColor3= Color3.fromRGB(35, 35, 35)
loadDivider.BorderSizePixel = 0
loadDivider.ZIndex          = 202
loadDivider.Parent          = loadCenter

-- Game name label
local loadGameName = Instance.new("TextLabel")
loadGameName.Size           = UDim2.fromOffset(340, 20)
loadGameName.Position       = UDim2.fromOffset(0, 124)
loadGameName.BackgroundTransparency = 1
loadGameName.Text           = gameName
loadGameName.TextColor3     = Color3.fromRGB(160, 160, 160)
loadGameName.TextSize       = 12
loadGameName.Font           = Enum.Font.GothamMedium
loadGameName.TextXAlignment = Enum.TextXAlignment.Center
loadGameName.TextTruncate   = Enum.TextTruncate.AtEnd
loadGameName.ZIndex         = 202
loadGameName.Parent         = loadCenter

-- Creator label
local loadCreator = Instance.new("TextLabel")
loadCreator.Size            = UDim2.fromOffset(340, 16)
loadCreator.Position        = UDim2.fromOffset(0, 144)
loadCreator.BackgroundTransparency = 1
loadCreator.Text            = "by " .. gameCreator
loadCreator.TextColor3      = Color3.fromRGB(65, 65, 65)
loadCreator.TextSize        = 10
loadCreator.Font            = Enum.Font.Gotham
loadCreator.TextXAlignment  = Enum.TextXAlignment.Center
loadCreator.ZIndex          = 202
loadCreator.Parent          = loadCenter

-- Progress bar track
local barTrack = Instance.new("Frame")
barTrack.Size               = UDim2.fromOffset(260, 3)
barTrack.Position           = UDim2.fromOffset(40, 172)
barTrack.BackgroundColor3   = Color3.fromRGB(30, 30, 30)
barTrack.BorderSizePixel    = 0
barTrack.ZIndex             = 202
barTrack.Parent             = loadCenter
local barTrackCorner = Instance.new("UICorner")
barTrackCorner.CornerRadius = UDim.new(0, 2)
barTrackCorner.Parent       = barTrack

-- Progress bar fill
local barFill = Instance.new("Frame")
barFill.Size               = UDim2.fromOffset(0, 3)
barFill.BackgroundColor3   = Color3.fromRGB(60, 220, 120)
barFill.BorderSizePixel    = 0
barFill.ZIndex             = 203
barFill.Parent             = barTrack
local barFillCorner = Instance.new("UICorner")
barFillCorner.CornerRadius = UDim.new(0, 2)
barFillCorner.Parent       = barFill

-- Status text
local loadStatus = Instance.new("TextLabel")
loadStatus.Size             = UDim2.fromOffset(320, 20)
loadStatus.Position         = UDim2.fromOffset(0, 185)
loadStatus.BackgroundTransparency = 1
loadStatus.Text             = "Initializing..."
loadStatus.TextColor3       = Color3.fromRGB(70, 70, 70)
loadStatus.TextSize         = 11
loadStatus.Font             = Enum.Font.Gotham
loadStatus.TextXAlignment   = Enum.TextXAlignment.Center
loadStatus.ZIndex           = 202
loadStatus.Parent           = loadCenter

-- Animated dots after status
local dotsLabel = Instance.new("TextLabel")
dotsLabel.Size              = UDim2.fromOffset(30, 20)
dotsLabel.Position          = UDim2.fromOffset(196, 185)
dotsLabel.BackgroundTransparency = 1
dotsLabel.Text              = ""
dotsLabel.TextColor3        = Color3.fromRGB(60, 130, 255)
dotsLabel.TextSize          = 11
dotsLabel.Font              = Enum.Font.GothamBold
dotsLabel.TextXAlignment    = Enum.TextXAlignment.Left
dotsLabel.ZIndex            = 203
dotsLabel.Parent            = loadCenter

-- Loading steps  (~4 seconds total across 7 steps)
local LOAD_STEPS = {
    { text = "Initializing",        pct = 0.05, wait = 0.40 },
    { text = "Loading theme",       pct = 0.20, wait = 0.55 },
    { text = "Building UI",         pct = 0.42, wait = 0.65 },
    { text = "Connecting services", pct = 0.60, wait = 0.60 },
    { text = "Loading tabs",        pct = 0.76, wait = 0.55 },
    { text = "Almost ready",        pct = 0.92, wait = 0.50 },
    { text = "Done",                pct = 1.00, wait = 0.25 },
}

-- Pulse the <> logo color while loading
local logoPulseRunning = true
task.spawn(function()
    local t = 0
    while logoPulseRunning do
        t += 0.05
        local brightness = 0.6 + 0.4 * math.sin(t * 3)
        loadLogo.TextColor3 = Color3.fromRGB(
            math.floor(60  * brightness),
            math.floor(220 * brightness),
            math.floor(120 * brightness)
        )
        task.wait(0.03)
    end
    loadLogo.TextColor3 = Color3.fromRGB(60, 220, 120)
end)

-- Animate dots while loading
local dotsRunning = true
task.spawn(function()
    local frames = {"", ".", "..", "..."}
    local i = 1
    while dotsRunning do
        dotsLabel.Text = frames[i]
        i = (i % #frames) + 1
        task.wait(0.3)
    end
    dotsLabel.Text = ""
end)

-- Run through loading steps
local function runLoadingSequence(onComplete)
    for _, step in ipairs(LOAD_STEPS) do
        loadStatus.Text = step.text
        -- Tween bar fill to target width
        local targetW = math.floor(260 * step.pct)
        TweenSvc:Create(barFill,
            TweenInfo.new(step.wait * 0.7, Enum.EasingStyle.Quart, Enum.EasingDirection.Out),
            { Size = UDim2.fromOffset(targetW, 3) }
        ):Play()
        task.wait(step.wait)
    end
    task.wait(0.2)
    onComplete()
end

-- This flag gates the rest of the script — main GUI is hidden until loading done
local _kernelReady = false
local _kernelReadyCallback = nil
local function waitForReady(cb)
    if _kernelReady then cb() else _kernelReadyCallback = cb end
end

-- Fade out loading screen and reveal main GUI
local function finishLoading(mainFrame)
    logoPulseRunning = false
    dotsRunning      = false
    loadStatus.Text  = "Done"

    -- Fade out
    TweenSvc:Create(loadScreen,
        TweenInfo.new(0.5, Enum.EasingStyle.Quart, Enum.EasingDirection.In),
        { BackgroundTransparency = 1 }
    ):Play()
    TweenSvc:Create(loadLogo,   TweenInfo.new(0.4), { TextTransparency = 1 }):Play()
    TweenSvc:Create(loadName,   TweenInfo.new(0.4), { TextTransparency = 1 }):Play()
    TweenSvc:Create(loadVersion,TweenInfo.new(0.4), { TextTransparency = 1 }):Play()
    TweenSvc:Create(loadStatus,   TweenInfo.new(0.4), { TextTransparency = 1 }):Play()
    TweenSvc:Create(loadGameName, TweenInfo.new(0.4), { TextTransparency = 1 }):Play()
    TweenSvc:Create(loadCreator,  TweenInfo.new(0.4), { TextTransparency = 1 }):Play()
    TweenSvc:Create(loadDivider,  TweenInfo.new(0.4), { BackgroundTransparency = 1 }):Play()
    TweenSvc:Create(barTrack,     TweenInfo.new(0.4), { BackgroundTransparency = 1 }):Play()
    TweenSvc:Create(barFill,      TweenInfo.new(0.4), { BackgroundTransparency = 1 }):Play()

    task.delay(0.5, function()
        loadScreen:Destroy()
        -- Fade main frame in
        mainFrame.BackgroundTransparency = 1
        mainFrame.Visible = true
        TweenSvc:Create(mainFrame,
            TweenInfo.new(0.35, Enum.EasingStyle.Quart, Enum.EasingDirection.Out),
            { BackgroundTransparency = 0 }
        ):Play()
    end)
end

-- Start loading sequence immediately (runs in background while GUI builds below)
task.spawn(function()
    runLoadingSequence(function()
        _kernelReady = true
        if _kernelReadyCallback then _kernelReadyCallback() end
    end)
end)

-- ─────────────────────────────────────────────────────────────
-- THEME  (dark slate + vivid green accents)
-- Backgrounds: near-black slate with very slight warm undertone
-- so the bright greens pop without feeling neon-on-green
-- ─────────────────────────────────────────────────────────────
local BG_MAIN         = Color3.fromRGB(18,  20,  24)   -- dark blue-slate (not green, complements green)
local BG_TITLEBAR     = Color3.fromRGB(14,  16,  20)   -- slightly darker slate header
local BG_CODEBOX      = Color3.fromRGB(11,  13,  16)   -- near-black editor
local BG_TOOLBAR      = Color3.fromRGB(14,  16,  20)   -- same as titlebar
local BG_BUTTON       = Color3.fromRGB(26,  29,  36)   -- slate button
local BG_BUTTON_HOVER = Color3.fromRGB(34,  40,  50)   -- hover brightened
local BG_PANEL        = Color3.fromRGB(16,  18,  22)   -- panel slightly lighter
local BG_PANEL_ITEM   = Color3.fromRGB(22,  25,  31)   -- list row
local BG_PANEL_HOVER  = Color3.fromRGB(28,  33,  42)   -- list row hover
local BG_INPUT        = Color3.fromRGB(20,  23,  28)   -- input bg
local BG_SETTINGS     = Color3.fromRGB(14,  16,  20)   -- same as titlebar
local TEXT_PRIMARY    = Color3.fromRGB(225, 238, 228)   -- soft white with just a hint of green
local TEXT_DIM        = Color3.fromRGB(95,  110, 125)   -- slate-grey muted
local TEXT_MUTED      = Color3.fromRGB(50,  58,  68)    -- very muted
local ACCENT_GREEN    = Color3.fromRGB(72,  225, 128)   -- vivid green — main accent
local ACCENT_TEAL     = Color3.fromRGB(48,  210, 180)   -- teal — secondary
local ACCENT_LIME     = Color3.fromRGB(170, 255, 100)   -- lime — highlights
local ACCENT_ORANGE   = Color3.fromRGB(255, 180, 60)    -- warnings
local ACCENT_RED      = Color3.fromRGB(225, 75,  75)    -- errors
local ACCENT_BLUE     = ACCENT_TEAL                     -- compat alias
local DIVIDER         = Color3.fromRGB(28,  32,  40)    -- slate divider
local TAB_ACTIVE_BG   = Color3.fromRGB(18,  20,  24)
local TAB_INACTIVE_BG = Color3.fromRGB(14,  16,  20)

-- ─────────────────────────────────────────────────────────────
-- LAYOUT
-- ─────────────────────────────────────────────────────────────
local WINDOW_W   = 870
local WINDOW_H   = 560
local TITLEBAR_H = 38
local TABBAR_H   = 34
local TOOLBAR_H  = 46
local EDITOR_TOP = TITLEBAR_H + TABBAR_H
local EDITOR_H   = WINDOW_H - EDITOR_TOP - TOOLBAR_H
local GUTTER_W   = 42

-- ─────────────────────────────────────────────────────────────
-- UTILITIES
-- ─────────────────────────────────────────────────────────────
local function addCorner(obj, r)
    local c = Instance.new("UICorner"); c.CornerRadius = UDim.new(0, r); c.Parent = obj
end
local function addStroke(obj, col, thick)
    local s = Instance.new("UIStroke"); s.Color = col or DIVIDER; s.Thickness = thick or 1; s.Parent = obj
end
local function hline(parent, yOff, zi)
    local d = Instance.new("Frame")
    d.Size = UDim2.new(1,0,0,1); d.Position = UDim2.fromOffset(0, yOff)
    d.BackgroundColor3 = DIVIDER; d.BorderSizePixel = 0; d.ZIndex = zi or 3; d.Parent = parent
    return d
end
local function tween(obj, t, props)
    TweenSvc:Create(obj, TweenInfo.new(t, Enum.EasingStyle.Quart, Enum.EasingDirection.Out), props):Play()
end

-- ─────────────────────────────────────────────────────────────
-- KEYBIND STATE  (default K)
-- ─────────────────────────────────────────────────────────────
local toggleKeybind = Enum.KeyCode.K   -- changed in settings

-- ─────────────────────────────────────────────────────────────
-- MAIN FRAME
-- ─────────────────────────────────────────────────────────────
local frame = Instance.new("Frame")
frame.Name             = "Main"
frame.Size             = UDim2.fromOffset(WINDOW_W, WINDOW_H)
frame.Position         = UDim2.fromScale(0.5, 0.5)
frame.AnchorPoint      = Vector2.new(0.5, 0.5)
frame.BackgroundColor3 = BG_MAIN
frame.BorderSizePixel  = 0
frame.ClipsDescendants = true
frame.Visible          = false  -- hidden until loading finishes
frame.Parent           = gui
addCorner(frame, 7)
addStroke(frame, Color3.fromRGB(25,55,38), 1)

local shadow = Instance.new("Frame")
shadow.Size                  = UDim2.new(1,16,1,16)
shadow.Position               = UDim2.fromOffset(-8,8)
shadow.BackgroundColor3       = Color3.fromRGB(0,0,0)
shadow.BackgroundTransparency = 0.5
shadow.BorderSizePixel        = 0
shadow.ZIndex                 = frame.ZIndex - 1
shadow.Parent                 = frame
addCorner(shadow, 10)

-- ─────────────────────────────────────────────────────────────
-- TITLE BAR
-- ─────────────────────────────────────────────────────────────
local titleBar = Instance.new("Frame")
titleBar.Name             = "TitleBar"
titleBar.Size             = UDim2.new(1,0,0,TITLEBAR_H)
titleBar.BackgroundColor3 = BG_TITLEBAR
titleBar.BorderSizePixel  = 0
titleBar.ZIndex           = 10
titleBar.Parent           = frame
hline(titleBar, TITLEBAR_H-1, 11)

local appIcon = Instance.new("TextLabel")
appIcon.Size = UDim2.fromOffset(30, TITLEBAR_H); appIcon.Position = UDim2.fromOffset(10,0)
appIcon.BackgroundTransparency = 1; appIcon.Text = "<>"; appIcon.TextColor3 = ACCENT_GREEN
appIcon.TextSize = 15; appIcon.Font = Enum.Font.Code; appIcon.ZIndex = 11; appIcon.Parent = titleBar

local titleLabel = Instance.new("TextLabel")
titleLabel.Size = UDim2.fromOffset(120, TITLEBAR_H); titleLabel.Position = UDim2.fromOffset(42,0)
titleLabel.BackgroundTransparency = 1; titleLabel.Text = "Kernel"
titleLabel.TextColor3 = TEXT_PRIMARY; titleLabel.TextSize = 14; titleLabel.Font = Enum.Font.GothamMedium
titleLabel.TextXAlignment = Enum.TextXAlignment.Left; titleLabel.ZIndex = 11; titleLabel.Parent = titleBar

-- Center icons — 3 pill-shaped icon buttons
local ICON_BTN_W = 32
local ICON_BTN_H = 26
local ICON_GAP   = 6
local iconRowW   = (ICON_BTN_W * 3) + (ICON_GAP * 2)

local iconRow = Instance.new("Frame")
iconRow.Size = UDim2.fromOffset(iconRowW, TITLEBAR_H)
iconRow.Position = UDim2.new(0.5, -iconRowW/2, 0, 0)
iconRow.BackgroundTransparency = 1; iconRow.ZIndex = 11; iconRow.Parent = titleBar

local function makeIconBtn(icon, xOff, hoverColor)
    local btn = Instance.new("TextButton")
    btn.Size = UDim2.fromOffset(ICON_BTN_W, ICON_BTN_H)
    btn.Position = UDim2.new(0, xOff, 0.5, -ICON_BTN_H/2)
    btn.BackgroundColor3 = BG_BUTTON
    btn.BorderSizePixel = 0
    btn.Text = icon
    btn.TextColor3 = TEXT_DIM
    btn.TextSize = 15
    btn.Font = Enum.Font.GothamMedium
    btn.ZIndex = 12
    btn.Parent = iconRow
    addCorner(btn, 5)
    btn.MouseEnter:Connect(function()
        btn.TextColor3 = hoverColor
        btn.BackgroundColor3 = BG_BUTTON_HOVER
    end)
    btn.MouseLeave:Connect(function()
        btn.TextColor3 = TEXT_DIM
        btn.BackgroundColor3 = BG_BUTTON
    end)
    return btn
end

local settingsIconBtn = makeIconBtn("⚙", 0,                        ACCENT_LIME)
local newTabIconBtn   = makeIconBtn("⊕", ICON_BTN_W + ICON_GAP,    ACCENT_GREEN)
local renameIconBtn   = makeIconBtn("✏", (ICON_BTN_W + ICON_GAP)*2, ACCENT_TEAL)

-- Window control buttons
local controlsContainer = Instance.new("Frame")
controlsContainer.Size = UDim2.fromOffset(90, TITLEBAR_H); controlsContainer.Position = UDim2.new(1,-100,0,0)
controlsContainer.BackgroundTransparency = 1; controlsContainer.ZIndex = 11; controlsContainer.Parent = titleBar

local function makeWinCtrl(xOff, icon, col)
    local btn = Instance.new("TextButton")
    btn.Size = UDim2.fromOffset(28,28); btn.Position = UDim2.new(0,xOff,0.5,-14)
    btn.BackgroundColor3 = BG_BUTTON; btn.Text = icon; btn.TextColor3 = TEXT_DIM
    btn.TextSize = 13; btn.Font = Enum.Font.GothamMedium; btn.BorderSizePixel = 0
    btn.ZIndex = 12; btn.Parent = controlsContainer; addCorner(btn, 4)
    btn.MouseEnter:Connect(function() btn.TextColor3 = col end)
    btn.MouseLeave:Connect(function() btn.TextColor3 = TEXT_DIM end)
    return btn
end

local minimiseBtn = makeWinCtrl(0,  "─", ACCENT_ORANGE)
makeWinCtrl(31, "□", ACCENT_TEAL)
local closeBtn    = makeWinCtrl(62, "✕", ACCENT_RED)

-- ✕ destroys entire GUI
closeBtn.MouseButton1Click:Connect(function() gui:Destroy() end)

-- ─────────────────────────────────────────────────────────────
-- NOTIFICATION SYSTEM  (bottom-right corner, simple & reliable)
-- ─────────────────────────────────────────────────────────────

-- Notification ScreenGui — completely separate from main gui
-- so it works even when main frame is hidden
local notifGui = Instance.new("ScreenGui")
notifGui.Name           = "KernelNotifs"
notifGui.ResetOnSpawn   = false
notifGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
notifGui.IgnoreGuiInset = true
notifGui.Parent         = playerGui

-- Sound — parented to a Part inside workspace so it reliably plays
local notifSound = Instance.new("Sound")
notifSound.SoundId  = "rbxassetid://9068386612"  -- soft UI pop
notifSound.Volume   = 0.6
notifSound.Parent   = game:GetService("SoundService")

-- Notification container — stacks from bottom-right upward
local notifContainer = Instance.new("Frame")
notifContainer.Name              = "NotifContainer"
notifContainer.Size              = UDim2.fromOffset(260, 400)
notifContainer.Position          = UDim2.new(1, -276, 1, -420)
notifContainer.BackgroundTransparency = 1
notifContainer.BorderSizePixel   = 0
notifContainer.ZIndex            = 200
notifContainer.Parent            = notifGui

local notifLayout = Instance.new("UIListLayout")
notifLayout.FillDirection        = Enum.FillDirection.Vertical
notifLayout.VerticalAlignment    = Enum.VerticalAlignment.Bottom
notifLayout.SortOrder            = Enum.SortOrder.LayoutOrder
notifLayout.Padding              = UDim.new(0, 6)
notifLayout.Parent               = notifContainer

local notifCounter = 0

local function showToast(msg, color)
    local col = color or ACCENT_GREEN
    notifCounter += 1

    -- Play sound
    notifSound:Play()

    -- Card
    local card = Instance.new("Frame")
    card.Name              = "Notif_" .. notifCounter
    card.Size              = UDim2.fromOffset(260, 44)
    card.BackgroundColor3  = Color3.fromRGB(20, 23, 20)
    card.BorderSizePixel   = 0
    card.LayoutOrder       = notifCounter
    card.ZIndex            = 201
    card.ClipsDescendants  = false
    -- Start off to the right
    card.Position          = UDim2.fromOffset(280, 0)
    card.Parent            = notifContainer
    addCorner(card, 7)

    -- Green-tinted stroke matching accent
    local cardStroke = Instance.new("UIStroke")
    cardStroke.Color     = col
    cardStroke.Thickness = 1
    cardStroke.Transparency = 0.6
    cardStroke.Parent    = card

    -- Left accent bar
    local bar = Instance.new("Frame")
    bar.Size             = UDim2.fromOffset(3, 26)
    bar.Position         = UDim2.fromOffset(9, 9)
    bar.BackgroundColor3 = col
    bar.BorderSizePixel  = 0
    bar.ZIndex           = 202
    bar.Parent           = card
    addCorner(bar, 2)

    -- Header "Kernel"
    local hdr = Instance.new("TextLabel")
    hdr.Size              = UDim2.new(1, -30, 0, 16)
    hdr.Position          = UDim2.fromOffset(20, 6)
    hdr.BackgroundTransparency = 1
    hdr.Text              = "Kernel"
    hdr.TextColor3        = col
    hdr.TextSize          = 9
    hdr.Font              = Enum.Font.GothamBold
    hdr.TextXAlignment    = Enum.TextXAlignment.Left
    hdr.ZIndex            = 202
    hdr.Parent            = card

    -- Message
    local lbl = Instance.new("TextLabel")
    lbl.Size              = UDim2.new(1, -30, 0, 18)
    lbl.Position          = UDim2.fromOffset(20, 20)
    lbl.BackgroundTransparency = 1
    lbl.Text              = msg
    lbl.TextColor3        = Color3.fromRGB(200, 215, 200)
    lbl.TextSize          = 11
    lbl.Font              = Enum.Font.Gotham
    lbl.TextXAlignment    = Enum.TextXAlignment.Left
    lbl.TextTruncate      = Enum.TextTruncate.AtEnd
    lbl.ZIndex            = 202
    lbl.Parent            = card

    -- Slide in from right
    TweenSvc:Create(card,
        TweenInfo.new(0.3, Enum.EasingStyle.Quart, Enum.EasingDirection.Out),
        { Position = UDim2.fromOffset(0, 0) }
    ):Play()

    -- Auto dismiss after 3 seconds
    task.delay(3, function()
        TweenSvc:Create(card,
            TweenInfo.new(0.25, Enum.EasingStyle.Quart, Enum.EasingDirection.In),
            { Position = UDim2.fromOffset(280, 0) }
        ):Play()
        task.delay(0.3, function()
            card:Destroy()
        end)
    end)
end

-- ─────────────────────────────────────────────────────────────
-- MINIMISE / TOGGLE
-- ─────────────────────────────────────────────────────────────
local isMinimised = false

local function setGuiVisible(visible)
    isMinimised = not visible
    frame.Visible = visible
end

local function getKeybindName()
    local name = tostring(toggleKeybind)
    return name:gsub("Enum%.KeyCode%.", "")
end

minimiseBtn.MouseButton1Click:Connect(function()
    setGuiVisible(false)
    -- Toast is parented to gui not frame, so it still shows when frame is hidden
    showToast("Press " .. getKeybindName() .. " to reopen Kernel", ACCENT_ORANGE)
end)

-- Global keybind to re-show
UIS.InputBegan:Connect(function(input, gpe)
    if gpe then return end
    if input.KeyCode == toggleKeybind and isMinimised then
        setGuiVisible(true)
    end
end)

-- ─────────────────────────────────────────────────────────────
-- TAB BAR
-- ─────────────────────────────────────────────────────────────
local tabs          = {}
local activeTabIndex = 0
local tabIdCounter  = 0

local tabBar = Instance.new("Frame")
tabBar.Name = "TabBar"; tabBar.Size = UDim2.new(1,0,0,TABBAR_H)
tabBar.Position = UDim2.fromOffset(0, TITLEBAR_H)
tabBar.BackgroundColor3 = BG_TITLEBAR; tabBar.BorderSizePixel = 0
tabBar.ZIndex = 9; tabBar.Parent = frame
hline(tabBar, TABBAR_H-1, 10)

local tabScroll = Instance.new("ScrollingFrame")
tabScroll.Name = "TabScroll"; tabScroll.Size = UDim2.new(1,-34,1,0)
tabScroll.BackgroundTransparency = 1; tabScroll.BorderSizePixel = 0
tabScroll.ScrollBarThickness = 0; tabScroll.ScrollingDirection = Enum.ScrollingDirection.X
tabScroll.CanvasSize = UDim2.fromOffset(0,0); tabScroll.ZIndex = 10; tabScroll.Parent = tabBar

local tabLayout = Instance.new("UIListLayout")
tabLayout.FillDirection = Enum.FillDirection.Horizontal
tabLayout.SortOrder = Enum.SortOrder.LayoutOrder
tabLayout.Padding = UDim.new(0,0); tabLayout.Parent = tabScroll

local newTabBtn = Instance.new("TextButton")
newTabBtn.Size = UDim2.fromOffset(34, TABBAR_H); newTabBtn.Position = UDim2.new(1,-34,0,0)
newTabBtn.BackgroundTransparency = 1; newTabBtn.Text = "+"
newTabBtn.TextColor3 = TEXT_DIM; newTabBtn.TextSize = 20; newTabBtn.Font = Enum.Font.GothamMedium
newTabBtn.ZIndex = 10; newTabBtn.Parent = tabBar
newTabBtn.MouseEnter:Connect(function() newTabBtn.TextColor3 = TEXT_PRIMARY end)
newTabBtn.MouseLeave:Connect(function() newTabBtn.TextColor3 = TEXT_DIM end)

-- ─────────────────────────────────────────────────────────────
-- GUTTER
-- ─────────────────────────────────────────────────────────────
local gutter = Instance.new("Frame")
gutter.Name = "Gutter"; gutter.Size = UDim2.fromOffset(GUTTER_W, EDITOR_H)
gutter.Position = UDim2.fromOffset(0, EDITOR_TOP)
gutter.BackgroundColor3 = BG_CODEBOX; gutter.BorderSizePixel = 0; gutter.ZIndex = 4; gutter.Parent = frame

local gutterBorder = Instance.new("Frame")
gutterBorder.Size = UDim2.fromOffset(1, EDITOR_H); gutterBorder.Position = UDim2.fromOffset(GUTTER_W, EDITOR_TOP)
gutterBorder.BackgroundColor3 = DIVIDER; gutterBorder.BorderSizePixel = 0; gutterBorder.ZIndex = 5; gutterBorder.Parent = frame

for i = 1, 30 do
    local ln = Instance.new("TextLabel")
    ln.Size = UDim2.fromOffset(GUTTER_W-6, 20); ln.Position = UDim2.fromOffset(0,(i-1)*20+6)
    ln.BackgroundTransparency = 1; ln.Text = tostring(i); ln.TextColor3 = Color3.fromRGB(40, 75, 55)
    ln.TextSize = 13; ln.Font = Enum.Font.Code; ln.TextXAlignment = Enum.TextXAlignment.Right
    ln.ZIndex = 5; ln.Parent = gutter
end

-- ─────────────────────────────────────────────────────────────
-- CODE EDITOR
-- ─────────────────────────────────────────────────────────────
local codeBox = Instance.new("TextBox")
codeBox.Name = "CodeBox"
codeBox.Size = UDim2.fromOffset(WINDOW_W - GUTTER_W - 1, EDITOR_H)
codeBox.Position = UDim2.fromOffset(GUTTER_W + 1, EDITOR_TOP)
codeBox.BackgroundColor3 = BG_CODEBOX; codeBox.TextColor3 = Color3.fromRGB(140, 240, 160)
codeBox.MultiLine = true; codeBox.ClearTextOnFocus = false
codeBox.TextXAlignment = Enum.TextXAlignment.Left; codeBox.TextYAlignment = Enum.TextYAlignment.Top
codeBox.TextSize = 14; codeBox.Font = Enum.Font.Code
codeBox.Text = ""; codeBox.BorderSizePixel = 0; codeBox.ZIndex = 4; codeBox.Parent = frame
local codePad = Instance.new("UIPadding")
codePad.PaddingLeft = UDim.new(0,8); codePad.PaddingTop = UDim.new(0,6); codePad.Parent = codeBox

-- ─────────────────────────────────────────────────────────────
-- OUTPUT BOX
-- ─────────────────────────────────────────────────────────────
local outputBox = Instance.new("TextBox")
outputBox.Name = "OutputBox"
outputBox.Size = UDim2.fromOffset(WINDOW_W - GUTTER_W - 1, 110)
outputBox.Position = UDim2.fromOffset(GUTTER_W+1, EDITOR_TOP + EDITOR_H - 110)
outputBox.BackgroundColor3 = Color3.fromRGB(10,10,10); outputBox.TextColor3 = Color3.fromRGB(100, 200, 130)
outputBox.MultiLine = true; outputBox.ClearTextOnFocus = false; outputBox.TextEditable = false
outputBox.TextWrapped = false; outputBox.TextXAlignment = Enum.TextXAlignment.Left
outputBox.TextYAlignment = Enum.TextYAlignment.Top; outputBox.Text = "Output:\n"
outputBox.TextSize = 13; outputBox.Font = Enum.Font.Code
outputBox.BorderSizePixel = 0; outputBox.ZIndex = 6; outputBox.Visible = false; outputBox.Parent = frame
local outPad = Instance.new("UIPadding")
outPad.PaddingLeft = UDim.new(0,8); outPad.PaddingTop = UDim.new(0,4); outPad.Parent = outputBox
local outBorder = Instance.new("Frame")
outBorder.Size = UDim2.new(1,0,0,1); outBorder.BackgroundColor3 = DIVIDER
outBorder.BorderSizePixel = 0; outBorder.ZIndex = 7; outBorder.Parent = outputBox

-- ─────────────────────────────────────────────────────────────
-- TOOLBAR
-- ─────────────────────────────────────────────────────────────
local toolbar = Instance.new("Frame")
toolbar.Name = "Toolbar"; toolbar.Size = UDim2.new(1,0,0,TOOLBAR_H)
toolbar.Position = UDim2.new(0,0,1,-TOOLBAR_H)
toolbar.BackgroundColor3 = BG_TOOLBAR; toolbar.BorderSizePixel = 0; toolbar.ZIndex = 8; toolbar.Parent = frame
hline(toolbar, 0, 9)

local function makeToolbarButton(icon, label, xOff, accentColor)
    local btn = Instance.new("TextButton")
    btn.Size = UDim2.fromOffset(94,34); btn.Position = UDim2.new(0,xOff,0.5,-17)
    btn.BackgroundColor3 = BG_BUTTON; btn.BorderSizePixel = 0; btn.Text = ""
    btn.ZIndex = 9; btn.Parent = toolbar; addCorner(btn, 5)
    local iconLbl = Instance.new("TextLabel")
    iconLbl.Size = UDim2.fromOffset(24,34); iconLbl.Position = UDim2.fromOffset(8,0)
    iconLbl.BackgroundTransparency = 1; iconLbl.Text = icon
    iconLbl.TextColor3 = accentColor or TEXT_DIM; iconLbl.TextSize = 14
    iconLbl.Font = Enum.Font.GothamMedium; iconLbl.ZIndex = 10; iconLbl.Parent = btn
    local textLbl = Instance.new("TextLabel")
    textLbl.Size = UDim2.new(1,-34,1,0); textLbl.Position = UDim2.fromOffset(32,0)
    textLbl.BackgroundTransparency = 1; textLbl.Text = label; textLbl.TextColor3 = TEXT_PRIMARY
    textLbl.TextSize = 13; textLbl.Font = Enum.Font.GothamMedium
    textLbl.TextXAlignment = Enum.TextXAlignment.Left; textLbl.ZIndex = 10; textLbl.Parent = btn
    btn.MouseEnter:Connect(function() btn.BackgroundColor3 = BG_BUTTON_HOVER end)
    btn.MouseLeave:Connect(function() btn.BackgroundColor3 = BG_BUTTON end)
    return btn
end

local executeButton = makeToolbarButton("▶","Execute", 8,   ACCENT_GREEN)
local clearButton   = makeToolbarButton("◇","Clear",   108, nil)
local openButton    = makeToolbarButton("⬒","Open",    208, nil)
local saveButton    = makeToolbarButton("⬓","Save",    308, nil)

-- Attach button
local attachButton = Instance.new("TextButton")
attachButton.Size = UDim2.fromOffset(114,34); attachButton.Position = UDim2.new(1,-274,0.5,-17)
attachButton.BackgroundColor3 = BG_BUTTON; attachButton.BorderSizePixel = 0
attachButton.Text = ""; attachButton.ZIndex = 9; attachButton.Parent = toolbar; addCorner(attachButton, 5)

local signalIcon = Instance.new("TextLabel")
signalIcon.Size = UDim2.fromOffset(28,34); signalIcon.Position = UDim2.fromOffset(6,0)
signalIcon.BackgroundTransparency = 1; signalIcon.Text = "((·))"; signalIcon.TextColor3 = ACCENT_RED
signalIcon.TextSize = 10; signalIcon.Font = Enum.Font.Code; signalIcon.ZIndex = 10; signalIcon.Parent = attachButton

local attachLabel = Instance.new("TextLabel")
attachLabel.Size = UDim2.fromOffset(76,34); attachLabel.Position = UDim2.fromOffset(34,0)
attachLabel.BackgroundTransparency = 1; attachLabel.Text = "Attach"; attachLabel.TextColor3 = TEXT_PRIMARY
attachLabel.TextSize = 13; attachLabel.Font = Enum.Font.GothamMedium
attachLabel.TextXAlignment = Enum.TextXAlignment.Left; attachLabel.ZIndex = 10; attachLabel.Parent = attachButton
attachButton.MouseEnter:Connect(function() attachButton.BackgroundColor3 = BG_BUTTON_HOVER end)
attachButton.MouseLeave:Connect(function() attachButton.BackgroundColor3 = BG_BUTTON end)

-- Status dot
local statusDot = Instance.new("Frame")
statusDot.Size = UDim2.fromOffset(8,8); statusDot.Position = UDim2.new(1,-154,0.5,-4)
statusDot.BackgroundColor3 = ACCENT_RED; statusDot.BorderSizePixel = 0
statusDot.ZIndex = 9; statusDot.Parent = toolbar; addCorner(statusDot, 4)

-- Player count
local playerCountFrame = Instance.new("Frame")
playerCountFrame.Size = UDim2.fromOffset(118,34); playerCountFrame.Position = UDim2.new(1,-130,0.5,-17)
playerCountFrame.BackgroundColor3 = BG_BUTTON; playerCountFrame.BorderSizePixel = 0
playerCountFrame.ZIndex = 9; playerCountFrame.Parent = toolbar; addCorner(playerCountFrame, 5)

local personIcon = Instance.new("TextLabel")
personIcon.Size = UDim2.fromOffset(22,34); personIcon.Position = UDim2.fromOffset(6,0)
personIcon.BackgroundTransparency = 1; personIcon.Text = "👤"; personIcon.TextSize = 13
personIcon.ZIndex = 10; personIcon.Parent = playerCountFrame

local playerCountLabel = Instance.new("TextLabel")
playerCountLabel.Size = UDim2.fromOffset(70,34); playerCountLabel.Position = UDim2.fromOffset(26,0)
playerCountLabel.BackgroundTransparency = 1; playerCountLabel.TextColor3 = TEXT_PRIMARY
playerCountLabel.TextSize = 13; playerCountLabel.Font = Enum.Font.GothamMedium
playerCountLabel.TextXAlignment = Enum.TextXAlignment.Left; playerCountLabel.ZIndex = 10
playerCountLabel.Parent = playerCountFrame

local caretLabel = Instance.new("TextLabel")
caretLabel.Size = UDim2.fromOffset(16,34); caretLabel.Position = UDim2.new(1,-18,0,0)
caretLabel.BackgroundTransparency = 1; caretLabel.Text = "▾"; caretLabel.TextColor3 = TEXT_DIM
caretLabel.TextSize = 12; caretLabel.ZIndex = 10; caretLabel.Parent = playerCountFrame

local function updatePlayerCount()
    playerCountLabel.Text = tostring(#Players:GetPlayers())
end
updatePlayerCount()
Players.PlayerAdded:Connect(updatePlayerCount)
Players.PlayerRemoving:Connect(updatePlayerCount)

-- ─────────────────────────────────────────────────────────────
-- SCRIPT DATABASE
-- ─────────────────────────────────────────────────────────────
local scriptDB = {}
local function dbSave(name, content)
    scriptDB[name] = { name=name, content=content, savedAt=os.date("%H:%M:%S") }
end
local function dbDelete(name) scriptDB[name] = nil end
local function dbList()
    local list = {}
    for _, v in pairs(scriptDB) do table.insert(list, v) end
    table.sort(list, function(a,b) return a.name < b.name end)
    return list
end

-- ─────────────────────────────────────────────────────────────
-- OVERLAY HELPER
-- ─────────────────────────────────────────────────────────────
local function makeOverlay(zi)
    local ov = Instance.new("TextButton")
    ov.Size = UDim2.fromScale(1,1); ov.BackgroundColor3 = Color3.fromRGB(0,0,0)
    ov.BackgroundTransparency = 0.5; ov.BorderSizePixel = 0; ov.ZIndex = zi
    ov.Text = ""; ov.AutoButtonColor = false; ov.Parent = frame
    return ov
end

-- ─────────────────────────────────────────────────────────────
-- TAB MANAGEMENT
-- ─────────────────────────────────────────────────────────────
local function saveActiveTabContent()
    if activeTabIndex > 0 and tabs[activeTabIndex] then
        tabs[activeTabIndex].content = codeBox.Text
    end
end

local function refreshTabBar()
    for _, child in ipairs(tabScroll:GetChildren()) do
        if not child:IsA("UIListLayout") then child:Destroy() end
    end
    local totalW = 0
    local TAB_W = 138
    for i, tab in ipairs(tabs) do
        local isActive = (i == activeTabIndex)
        local tabFrame = Instance.new("Frame")
        tabFrame.Name = "Tab_"..i; tabFrame.Size = UDim2.fromOffset(TAB_W, TABBAR_H)
        tabFrame.BackgroundColor3 = isActive and TAB_ACTIVE_BG or TAB_INACTIVE_BG
        tabFrame.BorderSizePixel = 0; tabFrame.LayoutOrder = i
        tabFrame.ZIndex = isActive and 11 or 10; tabFrame.Parent = tabScroll

        if isActive then
            local accent = Instance.new("Frame")
            accent.Size = UDim2.new(1,0,0,2); accent.Position = UDim2.new(0,0,1,-2)
            accent.BackgroundColor3 = ACCENT_GREEN; accent.BorderSizePixel = 0
            accent.ZIndex = 12; accent.Parent = tabFrame
        end

        local sep = Instance.new("Frame")
        sep.Size = UDim2.fromOffset(1,TABBAR_H); sep.Position = UDim2.new(1,-1,0,0)
        sep.BackgroundColor3 = DIVIDER; sep.BorderSizePixel = 0; sep.ZIndex = 11; sep.Parent = tabFrame

        local tIcon = Instance.new("TextLabel")
        tIcon.Size = UDim2.fromOffset(18,TABBAR_H); tIcon.Position = UDim2.fromOffset(8,0)
        tIcon.BackgroundTransparency = 1; tIcon.Text = "66"
        tIcon.TextColor3 = isActive and ACCENT_GREEN or TEXT_DIM
        tIcon.TextSize = 10; tIcon.Font = Enum.Font.GothamBold; tIcon.ZIndex = 12; tIcon.Parent = tabFrame

        local tName = Instance.new("TextLabel")
        tName.Size = UDim2.fromOffset(88,TABBAR_H); tName.Position = UDim2.fromOffset(28,0)
        tName.BackgroundTransparency = 1; tName.Text = tab.name
        tName.TextColor3 = isActive and TEXT_PRIMARY or TEXT_DIM
        tName.TextSize = 12; tName.Font = Enum.Font.Gotham
        tName.TextXAlignment = Enum.TextXAlignment.Left; tName.TextTruncate = Enum.TextTruncate.AtEnd
        tName.ZIndex = 12; tName.Parent = tabFrame

        local tClose = Instance.new("TextButton")
        tClose.Size = UDim2.fromOffset(18,18); tClose.Position = UDim2.new(1,-22,0.5,-9)
        tClose.BackgroundColor3 = Color3.fromRGB(55,55,55); tClose.BackgroundTransparency = 1
        tClose.Text = "✕"; tClose.TextColor3 = TEXT_DIM; tClose.TextSize = 10
        tClose.Font = Enum.Font.GothamMedium; tClose.ZIndex = 13; tClose.Parent = tabFrame
        addCorner(tClose, 3)
        tClose.MouseEnter:Connect(function() tClose.BackgroundTransparency=0; tClose.TextColor3=TEXT_PRIMARY end)
        tClose.MouseLeave:Connect(function() tClose.BackgroundTransparency=1; tClose.TextColor3=TEXT_DIM end)

        local capturedI = i
        tClose.MouseButton1Click:Connect(function()
            saveActiveTabContent()
            table.remove(tabs, capturedI)
            if #tabs == 0 then
                tabIdCounter += 1
                table.insert(tabs, {name="Untitled "..tabIdCounter, content="-- type code here"})
                activeTabIndex = 1
            elseif activeTabIndex >= capturedI then
                activeTabIndex = math.max(1, activeTabIndex - 1)
            end
            codeBox.Text = tabs[activeTabIndex].content
            refreshTabBar()
        end)

        local clickZone = Instance.new("TextButton")
        clickZone.Size = UDim2.new(1,-22,1,0); clickZone.BackgroundTransparency = 1
        clickZone.Text = ""; clickZone.ZIndex = 11; clickZone.Parent = tabFrame

        local switchI = i
        clickZone.MouseButton1Click:Connect(function()
            if switchI == activeTabIndex then return end
            saveActiveTabContent()
            activeTabIndex = switchI
            codeBox.Text = tabs[activeTabIndex].content
            refreshTabBar()
        end)

        totalW += TAB_W
    end
    tabScroll.CanvasSize = UDim2.fromOffset(totalW, 0)
end

local function createNewTab(name, content)
    saveActiveTabContent()
    tabIdCounter += 1
    local tabName = name or ("Untitled "..tabIdCounter)
    table.insert(tabs, {name=tabName, content=content or "-- type code here"})
    activeTabIndex = #tabs
    codeBox.Text = tabs[activeTabIndex].content
    refreshTabBar()
end

createNewTab("Untitled 1", "-- type code here")
newTabBtn.MouseButton1Click:Connect(function() createNewTab() end)
-- ⊕ icon also creates a new tab
newTabIconBtn.MouseButton1Click:Connect(function() createNewTab() end)
codeBox:GetPropertyChangedSignal("Text"):Connect(function()
    if tabs[activeTabIndex] then tabs[activeTabIndex].content = codeBox.Text end
end)

-- ─────────────────────────────────────────────────────────────
-- RENAME TAB POPUP  (triggered by ✏ icon)
-- ─────────────────────────────────────────────────────────────
local renameOverlay, renamePopup

local function closeRenamePopup()
    if renameOverlay then renameOverlay:Destroy(); renameOverlay = nil end
    if renamePopup   then renamePopup:Destroy();   renamePopup   = nil end
end

local function openRenamePopup()
    if renamePopup then closeRenamePopup(); return end
    if not tabs[activeTabIndex] then return end

    renameOverlay = makeOverlay(30)
    renameOverlay.MouseButton1Click:Connect(closeRenamePopup)

    renamePopup = Instance.new("Frame")
    renamePopup.Size = UDim2.fromOffset(360, 160)
    renamePopup.Position = UDim2.fromScale(0.5, 0.5)
    renamePopup.AnchorPoint = Vector2.new(0.5, 0.5)
    renamePopup.BackgroundColor3 = BG_SETTINGS
    renamePopup.BorderSizePixel = 0; renamePopup.ZIndex = 35; renamePopup.Parent = frame
    addCorner(renamePopup, 8); addStroke(renamePopup, Color3.fromRGB(25,55,38), 1)

    -- Header
    local rpTitle = Instance.new("TextLabel")
    rpTitle.Size = UDim2.new(1,-16,0,44); rpTitle.Position = UDim2.fromOffset(16,0)
    rpTitle.BackgroundTransparency = 1; rpTitle.Text = "✏  Rename Tab"
    rpTitle.TextColor3 = TEXT_PRIMARY; rpTitle.TextSize = 15; rpTitle.Font = Enum.Font.GothamMedium
    rpTitle.TextXAlignment = Enum.TextXAlignment.Left; rpTitle.ZIndex = 36; rpTitle.Parent = renamePopup
    hline(renamePopup, 44, 36)

    local rpClose = Instance.new("TextButton")
    rpClose.Size = UDim2.fromOffset(28,28); rpClose.Position = UDim2.new(1,-40,0,8)
    rpClose.BackgroundColor3 = BG_BUTTON; rpClose.Text = "✕"; rpClose.TextColor3 = TEXT_DIM
    rpClose.TextSize = 13; rpClose.Font = Enum.Font.GothamMedium; rpClose.BorderSizePixel = 0
    rpClose.ZIndex = 36; rpClose.Parent = renamePopup; addCorner(rpClose, 4)
    rpClose.MouseButton1Click:Connect(closeRenamePopup)
    rpClose.MouseEnter:Connect(function() rpClose.TextColor3 = ACCENT_RED end)
    rpClose.MouseLeave:Connect(function() rpClose.TextColor3 = TEXT_DIM end)

    -- Input
    local rpInput = Instance.new("TextBox")
    rpInput.Size = UDim2.new(1,-32,0,36); rpInput.Position = UDim2.fromOffset(16,56)
    rpInput.BackgroundColor3 = BG_INPUT; rpInput.TextColor3 = TEXT_PRIMARY
    rpInput.TextSize = 14; rpInput.Font = Enum.Font.Gotham
    rpInput.PlaceholderText = "Enter tab name..."; rpInput.PlaceholderColor3 = TEXT_MUTED
    rpInput.Text = tabs[activeTabIndex].name
    rpInput.ClearTextOnFocus = false; rpInput.BorderSizePixel = 0
    rpInput.ZIndex = 36; rpInput.Parent = renamePopup
    addCorner(rpInput, 5); addStroke(rpInput, DIVIDER, 1)
    local rp = Instance.new("UIPadding"); rp.PaddingLeft = UDim.new(0,10); rp.Parent = rpInput

    local rpErr = Instance.new("TextLabel")
    rpErr.Size = UDim2.new(1,-32,0,16); rpErr.Position = UDim2.fromOffset(16,96)
    rpErr.BackgroundTransparency = 1; rpErr.Text = ""
    rpErr.TextColor3 = ACCENT_RED; rpErr.TextSize = 11; rpErr.Font = Enum.Font.Gotham
    rpErr.TextXAlignment = Enum.TextXAlignment.Left; rpErr.ZIndex = 36; rpErr.Parent = renamePopup

    -- Confirm button
    local rpConfirm = Instance.new("TextButton")
    rpConfirm.Size = UDim2.new(1,-32,0,34); rpConfirm.Position = UDim2.fromOffset(16,114)
    rpConfirm.BackgroundColor3 = Color3.fromRGB(20,90,70); rpConfirm.Text = "Rename"
    rpConfirm.TextColor3 = TEXT_PRIMARY; rpConfirm.TextSize = 13; rpConfirm.Font = Enum.Font.GothamMedium
    rpConfirm.BorderSizePixel = 0; rpConfirm.ZIndex = 36; rpConfirm.Parent = renamePopup
    addCorner(rpConfirm, 5)
    rpConfirm.MouseEnter:Connect(function() rpConfirm.BackgroundColor3 = Color3.fromRGB(28,110,85) end)
    rpConfirm.MouseLeave:Connect(function() rpConfirm.BackgroundColor3 = Color3.fromRGB(20,90,70) end)

    local function doRename()
        local name = rpInput.Text:match("^%s*(.-)%s*$")
        if name == "" then rpErr.Text = "⚠  Name cannot be empty."; return end
        tabs[activeTabIndex].name = name
        refreshTabBar()
        closeRenamePopup()
        showToast("Tab renamed to: " .. name, ACCENT_BLUE)
    end

    rpConfirm.MouseButton1Click:Connect(doRename)
    -- Also confirm on Enter key
    rpInput.FocusLost:Connect(function(enterPressed)
        if enterPressed then doRename() end
    end)

    -- Auto-focus the input
    task.defer(function() rpInput:CaptureFocus() end)
end

renameIconBtn.MouseButton1Click:Connect(openRenamePopup)

-- ─────────────────────────────────────────────────────────────
-- SETTINGS PANEL
-- ─────────────────────────────────────────────────────────────
local settingsOverlay, settingsPanel
local listeningForKey = false

local function closeSettings()
    if settingsOverlay then settingsOverlay:Destroy(); settingsOverlay = nil end
    if settingsPanel then settingsPanel:Destroy(); settingsPanel = nil end
    listeningForKey = false
end

local function openSettings()
    if settingsPanel then closeSettings(); return end

    settingsOverlay = makeOverlay(30)
    settingsOverlay.MouseButton1Click:Connect(closeSettings)

    settingsPanel = Instance.new("Frame")
    settingsPanel.Name = "Settings"; settingsPanel.Size = UDim2.fromOffset(420, 280)
    settingsPanel.Position = UDim2.fromScale(0.5, 0.5); settingsPanel.AnchorPoint = Vector2.new(0.5,0.5)
    settingsPanel.BackgroundColor3 = BG_SETTINGS; settingsPanel.BorderSizePixel = 0
    settingsPanel.ZIndex = 35; settingsPanel.Parent = frame
    addCorner(settingsPanel, 8); addStroke(settingsPanel, Color3.fromRGB(25,55,38), 1)

    -- Header
    local sTitle = Instance.new("TextLabel")
    sTitle.Size = UDim2.new(1,-16,0,44); sTitle.Position = UDim2.fromOffset(16,0)
    sTitle.BackgroundTransparency = 1; sTitle.Text = "⚙  Settings"
    sTitle.TextColor3 = TEXT_PRIMARY; sTitle.TextSize = 15; sTitle.Font = Enum.Font.GothamMedium
    sTitle.TextXAlignment = Enum.TextXAlignment.Left; sTitle.ZIndex = 36; sTitle.Parent = settingsPanel
    hline(settingsPanel, 44, 36)

    local sCloseBtn = Instance.new("TextButton")
    sCloseBtn.Size = UDim2.fromOffset(28,28); sCloseBtn.Position = UDim2.new(1,-40,0,8)
    sCloseBtn.BackgroundColor3 = BG_BUTTON; sCloseBtn.Text = "✕"; sCloseBtn.TextColor3 = TEXT_DIM
    sCloseBtn.TextSize = 13; sCloseBtn.Font = Enum.Font.GothamMedium; sCloseBtn.BorderSizePixel = 0
    sCloseBtn.ZIndex = 36; sCloseBtn.Parent = settingsPanel; addCorner(sCloseBtn, 4)
    sCloseBtn.MouseButton1Click:Connect(closeSettings)
    sCloseBtn.MouseEnter:Connect(function() sCloseBtn.TextColor3 = ACCENT_RED end)
    sCloseBtn.MouseLeave:Connect(function() sCloseBtn.TextColor3 = TEXT_DIM end)

    -- Section: Toggle Keybind
    local secLabel = Instance.new("TextLabel")
    secLabel.Size = UDim2.new(1,-32,0,20); secLabel.Position = UDim2.fromOffset(16,58)
    secLabel.BackgroundTransparency = 1; secLabel.Text = "TOGGLE KEYBIND"
    secLabel.TextColor3 = TEXT_MUTED; secLabel.TextSize = 10; secLabel.Font = Enum.Font.GothamBold
    secLabel.TextXAlignment = Enum.TextXAlignment.Left; secLabel.ZIndex = 36; secLabel.Parent = settingsPanel

    local keybindDesc = Instance.new("TextLabel")
    keybindDesc.Size = UDim2.new(1,-32,0,18); keybindDesc.Position = UDim2.fromOffset(16,80)
    keybindDesc.BackgroundTransparency = 1
    keybindDesc.Text = "Key used to show the executor when minimised"
    keybindDesc.TextColor3 = TEXT_DIM; keybindDesc.TextSize = 11; keybindDesc.Font = Enum.Font.Gotham
    keybindDesc.TextXAlignment = Enum.TextXAlignment.Left; keybindDesc.ZIndex = 36; keybindDesc.Parent = settingsPanel

    -- Keybind button
    local kbRow = Instance.new("Frame")
    kbRow.Size = UDim2.new(1,-32,0,40); kbRow.Position = UDim2.fromOffset(16,104)
    kbRow.BackgroundTransparency = 1; kbRow.ZIndex = 36; kbRow.Parent = settingsPanel

    local currentKeyLabel = Instance.new("TextLabel")
    currentKeyLabel.Size = UDim2.new(1,-120,1,0); currentKeyLabel.BackgroundTransparency = 1
    currentKeyLabel.Text = "Current key:"; currentKeyLabel.TextColor3 = TEXT_DIM
    currentKeyLabel.TextSize = 13; currentKeyLabel.Font = Enum.Font.Gotham
    currentKeyLabel.TextXAlignment = Enum.TextXAlignment.Left; currentKeyLabel.ZIndex = 37
    currentKeyLabel.Parent = kbRow

    local keyBadge = Instance.new("Frame")
    keyBadge.Size = UDim2.fromOffset(100,32); keyBadge.Position = UDim2.new(1,-100,0.5,-16)
    keyBadge.BackgroundColor3 = BG_INPUT; keyBadge.BorderSizePixel = 0; keyBadge.ZIndex = 37
    keyBadge.Parent = kbRow; addCorner(keyBadge, 6); addStroke(keyBadge, DIVIDER, 1)

    local keyText = Instance.new("TextLabel")
    keyText.Size = UDim2.new(1,0,1,0); keyText.BackgroundTransparency = 1
    keyText.Text = getKeybindName(); keyText.TextColor3 = ACCENT_BLUE
    keyText.TextSize = 14; keyText.Font = Enum.Font.GothamBold; keyText.ZIndex = 38; keyText.Parent = keyBadge

    -- Change keybind button
    local changeBtn = Instance.new("TextButton")
    changeBtn.Size = UDim2.new(1,-32,0,36); changeBtn.Position = UDim2.fromOffset(16,152)
    changeBtn.BackgroundColor3 = BG_BUTTON; changeBtn.BorderSizePixel = 0
    changeBtn.Text = "Click to change keybind"; changeBtn.TextColor3 = TEXT_PRIMARY
    changeBtn.TextSize = 13; changeBtn.Font = Enum.Font.GothamMedium; changeBtn.ZIndex = 36
    changeBtn.Parent = settingsPanel; addCorner(changeBtn, 5)
    changeBtn.MouseEnter:Connect(function() changeBtn.BackgroundColor3 = BG_BUTTON_HOVER end)
    changeBtn.MouseLeave:Connect(function() changeBtn.BackgroundColor3 = BG_BUTTON end)

    local noteLabel = Instance.new("TextLabel")
    noteLabel.Size = UDim2.new(1,-32,0,24); noteLabel.Position = UDim2.fromOffset(16,192)
    noteLabel.BackgroundTransparency = 1; noteLabel.Text = ""
    noteLabel.TextColor3 = TEXT_DIM; noteLabel.TextSize = 11; noteLabel.Font = Enum.Font.Gotham
    noteLabel.TextXAlignment = Enum.TextXAlignment.Left; noteLabel.ZIndex = 36; noteLabel.Parent = settingsPanel

    hline(settingsPanel, 226, 36)

    -- Info row
    local infoLabel = Instance.new("TextLabel")
    infoLabel.Size = UDim2.new(1,-32,0,40); infoLabel.Position = UDim2.fromOffset(16,232)
    infoLabel.BackgroundTransparency = 1
    infoLabel.Text = "Other settings coming soon."
    infoLabel.TextColor3 = TEXT_MUTED; infoLabel.TextSize = 11; infoLabel.Font = Enum.Font.Gotham
    infoLabel.TextXAlignment = Enum.TextXAlignment.Left; infoLabel.ZIndex = 36; infoLabel.Parent = settingsPanel

    -- Listen logic
    local keyInputConn
    changeBtn.MouseButton1Click:Connect(function()
        if listeningForKey then return end
        listeningForKey = true
        changeBtn.Text = "⬤  Press any key..."
        changeBtn.TextColor3 = ACCENT_ORANGE
        noteLabel.Text = "Press Escape to cancel"

        keyInputConn = UIS.InputBegan:Connect(function(input, gpe)
            if not listeningForKey then return end
            -- ignore mouse
            if input.UserInputType ~= Enum.UserInputType.Keyboard then return end
            if input.KeyCode == Enum.KeyCode.Escape then
                listeningForKey = false
                changeBtn.Text = "Click to change keybind"
                changeBtn.TextColor3 = TEXT_PRIMARY
                noteLabel.Text = "Cancelled."
                if keyInputConn then keyInputConn:Disconnect() end
                return
            end
            toggleKeybind = input.KeyCode
            listeningForKey = false
            changeBtn.Text = "Click to change keybind"
            changeBtn.TextColor3 = TEXT_PRIMARY
            keyText.Text = getKeybindName()
            noteLabel.Text = "✓ Keybind set to " .. getKeybindName()
            if keyInputConn then keyInputConn:Disconnect() end
        end)
    end)
end

settingsIconBtn.MouseButton1Click:Connect(openSettings)

-- ─────────────────────────────────────────────────────────────
-- SAVE PANEL
-- ─────────────────────────────────────────────────────────────
local saveOverlay, savePanel

local function closeSavePanel()
    if saveOverlay then saveOverlay:Destroy(); saveOverlay = nil end
    if savePanel   then savePanel:Destroy();  savePanel   = nil end
end

local function openSavePanel()
    if savePanel then closeSavePanel() end
    saveActiveTabContent()

    saveOverlay = makeOverlay(20)
    saveOverlay.MouseButton1Click:Connect(closeSavePanel)

    savePanel = Instance.new("Frame")
    savePanel.Size = UDim2.fromOffset(400,210); savePanel.Position = UDim2.fromScale(0.5,0.5)
    savePanel.AnchorPoint = Vector2.new(0.5,0.5); savePanel.BackgroundColor3 = BG_PANEL
    savePanel.BorderSizePixel = 0; savePanel.ZIndex = 25; savePanel.Parent = frame
    addCorner(savePanel, 8); addStroke(savePanel, Color3.fromRGB(25,55,38), 1)

    local spTitle = Instance.new("TextLabel")
    spTitle.Size = UDim2.new(1,-16,0,44); spTitle.Position = UDim2.fromOffset(16,0)
    spTitle.BackgroundTransparency = 1; spTitle.Text = "⬓  Save Script"
    spTitle.TextColor3 = TEXT_PRIMARY; spTitle.TextSize = 15; spTitle.Font = Enum.Font.GothamMedium
    spTitle.TextXAlignment = Enum.TextXAlignment.Left; spTitle.ZIndex = 26; spTitle.Parent = savePanel
    hline(savePanel, 44, 26)

    local spCloseBtn = Instance.new("TextButton")
    spCloseBtn.Size = UDim2.fromOffset(28,28); spCloseBtn.Position = UDim2.new(1,-40,0,8)
    spCloseBtn.BackgroundColor3 = BG_BUTTON; spCloseBtn.Text = "✕"; spCloseBtn.TextColor3 = TEXT_DIM
    spCloseBtn.TextSize = 13; spCloseBtn.Font = Enum.Font.GothamMedium; spCloseBtn.BorderSizePixel = 0
    spCloseBtn.ZIndex = 26; spCloseBtn.Parent = savePanel; addCorner(spCloseBtn, 4)
    spCloseBtn.MouseButton1Click:Connect(closeSavePanel)
    spCloseBtn.MouseEnter:Connect(function() spCloseBtn.TextColor3 = ACCENT_RED end)
    spCloseBtn.MouseLeave:Connect(function() spCloseBtn.TextColor3 = TEXT_DIM end)

    local inpLabel = Instance.new("TextLabel")
    inpLabel.Size = UDim2.new(1,-32,0,20); inpLabel.Position = UDim2.fromOffset(16,56)
    inpLabel.BackgroundTransparency = 1; inpLabel.Text = "Script name"
    inpLabel.TextColor3 = TEXT_DIM; inpLabel.TextSize = 11; inpLabel.Font = Enum.Font.Gotham
    inpLabel.TextXAlignment = Enum.TextXAlignment.Left; inpLabel.ZIndex = 26; inpLabel.Parent = savePanel

    local nameInput = Instance.new("TextBox")
    nameInput.Size = UDim2.new(1,-32,0,36); nameInput.Position = UDim2.fromOffset(16,78)
    nameInput.BackgroundColor3 = BG_INPUT; nameInput.TextColor3 = TEXT_PRIMARY
    nameInput.TextSize = 14; nameInput.Font = Enum.Font.Gotham
    nameInput.PlaceholderText = "Enter script name..."; nameInput.PlaceholderColor3 = TEXT_MUTED
    nameInput.Text = tabs[activeTabIndex] and tabs[activeTabIndex].name or "Untitled"
    nameInput.ClearTextOnFocus = false; nameInput.BorderSizePixel = 0
    nameInput.ZIndex = 26; nameInput.Parent = savePanel
    addCorner(nameInput, 5); addStroke(nameInput, DIVIDER, 1)
    local np = Instance.new("UIPadding"); np.PaddingLeft = UDim.new(0,10); np.Parent = nameInput

    local errLabel = Instance.new("TextLabel")
    errLabel.Size = UDim2.new(1,-32,0,18); errLabel.Position = UDim2.fromOffset(16,118)
    errLabel.BackgroundTransparency = 1; errLabel.Text = ""
    errLabel.TextColor3 = ACCENT_RED; errLabel.TextSize = 11; errLabel.Font = Enum.Font.Gotham
    errLabel.TextXAlignment = Enum.TextXAlignment.Left; errLabel.ZIndex = 26; errLabel.Parent = savePanel

    local cancelBtn = Instance.new("TextButton")
    cancelBtn.Size = UDim2.fromOffset(110,34); cancelBtn.Position = UDim2.fromOffset(16,160)
    cancelBtn.BackgroundColor3 = BG_BUTTON; cancelBtn.Text = "Cancel"; cancelBtn.TextColor3 = TEXT_DIM
    cancelBtn.TextSize = 13; cancelBtn.Font = Enum.Font.GothamMedium; cancelBtn.BorderSizePixel = 0
    cancelBtn.ZIndex = 26; cancelBtn.Parent = savePanel; addCorner(cancelBtn, 5)
    cancelBtn.MouseEnter:Connect(function() cancelBtn.BackgroundColor3 = BG_BUTTON_HOVER end)
    cancelBtn.MouseLeave:Connect(function() cancelBtn.BackgroundColor3 = BG_BUTTON end)
    cancelBtn.MouseButton1Click:Connect(closeSavePanel)

    local confirmBtn = Instance.new("TextButton")
    confirmBtn.Size = UDim2.fromOffset(238,34); confirmBtn.Position = UDim2.fromOffset(146,160)
    confirmBtn.BackgroundColor3 = Color3.fromRGB(20,90,70); confirmBtn.Text = "💾  Save to Database"
    confirmBtn.TextColor3 = TEXT_PRIMARY; confirmBtn.TextSize = 13; confirmBtn.Font = Enum.Font.GothamMedium
    confirmBtn.BorderSizePixel = 0; confirmBtn.ZIndex = 26; confirmBtn.Parent = savePanel; addCorner(confirmBtn, 5)
    confirmBtn.MouseEnter:Connect(function() confirmBtn.BackgroundColor3 = Color3.fromRGB(28,110,85) end)
    confirmBtn.MouseLeave:Connect(function() confirmBtn.BackgroundColor3 = Color3.fromRGB(20,90,70) end)

    confirmBtn.MouseButton1Click:Connect(function()
        local name = nameInput.Text:match("^%s*(.-)%s*$")
        if name == "" then errLabel.Text = "⚠  Name cannot be empty."; return end
        local code = tabs[activeTabIndex] and tabs[activeTabIndex].content or ""
        dbSave(name, code)
        if tabs[activeTabIndex] then tabs[activeTabIndex].name = name; refreshTabBar() end
        closeSavePanel()
        showToast("Saved: "..name, ACCENT_GREEN)
    end)
end

-- ─────────────────────────────────────────────────────────────
-- OPEN PANEL  (fixed: rebuilds cleanly every open)
-- ─────────────────────────────────────────────────────────────
local openOverlay, openPanel

local function closeOpenPanel()
    if openOverlay then openOverlay:Destroy(); openOverlay = nil end
    if openPanel   then openPanel:Destroy();  openPanel   = nil end
end

local function buildScriptList(scrollFrame, listLayout)
    -- Clear existing rows
    for _, child in ipairs(scrollFrame:GetChildren()) do
        if not child:IsA("UIListLayout") and not child:IsA("UIPadding") then
            child:Destroy()
        end
    end

    local scripts = dbList()

    if #scripts == 0 then
        local empty = Instance.new("Frame")
        empty.Size = UDim2.new(1,0,0,80); empty.BackgroundTransparency = 1
        empty.ZIndex = 27; empty.Parent = scrollFrame

        local emptyIcon = Instance.new("TextLabel")
        emptyIcon.Size = UDim2.new(1,0,0,30); emptyIcon.BackgroundTransparency = 1
        emptyIcon.Text = "{ }"; emptyIcon.TextColor3 = TEXT_MUTED; emptyIcon.TextSize = 20
        emptyIcon.Font = Enum.Font.Code; emptyIcon.ZIndex = 28; emptyIcon.Parent = empty

        local emptyText = Instance.new("TextLabel")
        emptyText.Size = UDim2.new(1,0,0,40); emptyText.Position = UDim2.fromOffset(0,32)
        emptyText.BackgroundTransparency = 1; emptyText.Text = "No saved scripts yet.  Use  Save  to store one."
        emptyText.TextColor3 = TEXT_DIM; emptyText.TextSize = 12; emptyText.Font = Enum.Font.Gotham
        emptyText.ZIndex = 28; emptyText.Parent = empty
    else
        for idx, script in ipairs(scripts) do
            local row = Instance.new("Frame")
            row.Name = "Row_"..idx; row.Size = UDim2.new(1,-4,0,50)
            row.BackgroundColor3 = BG_PANEL_ITEM; row.BorderSizePixel = 0
            row.LayoutOrder = idx; row.ZIndex = 27; row.Parent = scrollFrame; addCorner(row, 6)

            local sIcon = Instance.new("TextLabel")
            sIcon.Size = UDim2.fromOffset(36,50); sIcon.Position = UDim2.fromOffset(8,0)
            sIcon.BackgroundTransparency = 1; sIcon.Text = "{ }"; sIcon.TextColor3 = ACCENT_BLUE
            sIcon.TextSize = 12; sIcon.Font = Enum.Font.Code; sIcon.ZIndex = 28; sIcon.Parent = row

            local sName = Instance.new("TextLabel")
            sName.Size = UDim2.new(1,-160,0,26); sName.Position = UDim2.fromOffset(46,6)
            sName.BackgroundTransparency = 1; sName.Text = script.name
            sName.TextColor3 = TEXT_PRIMARY; sName.TextSize = 13; sName.Font = Enum.Font.GothamMedium
            sName.TextXAlignment = Enum.TextXAlignment.Left; sName.ZIndex = 28; sName.Parent = row

            local sMeta = Instance.new("TextLabel")
            sMeta.Size = UDim2.new(1,-160,0,16); sMeta.Position = UDim2.fromOffset(46,30)
            sMeta.BackgroundTransparency = 1
            local lc = 0; for _ in script.content:gmatch("\n") do lc += 1 end
            sMeta.Text = (lc+1).." lines  ·  saved "..script.savedAt
            sMeta.TextColor3 = TEXT_DIM; sMeta.TextSize = 10; sMeta.Font = Enum.Font.Gotham
            sMeta.TextXAlignment = Enum.TextXAlignment.Left; sMeta.ZIndex = 28; sMeta.Parent = row

            local loadBtn = Instance.new("TextButton")
            loadBtn.Size = UDim2.fromOffset(58,28); loadBtn.Position = UDim2.new(1,-96,0.5,-14)
            loadBtn.BackgroundColor3 = Color3.fromRGB(20,90,70); loadBtn.Text = "Load"
            loadBtn.TextColor3 = TEXT_PRIMARY; loadBtn.TextSize = 12; loadBtn.Font = Enum.Font.GothamMedium
            loadBtn.BorderSizePixel = 0; loadBtn.ZIndex = 29; loadBtn.Parent = row; addCorner(loadBtn, 5)
            loadBtn.MouseEnter:Connect(function() loadBtn.BackgroundColor3 = Color3.fromRGB(28,110,85) end)
            loadBtn.MouseLeave:Connect(function() loadBtn.BackgroundColor3 = Color3.fromRGB(20,90,70) end)

            local delBtn = Instance.new("TextButton")
            delBtn.Size = UDim2.fromOffset(28,28); delBtn.Position = UDim2.new(1,-34,0.5,-14)
            delBtn.BackgroundColor3 = Color3.fromRGB(60,20,20); delBtn.BackgroundTransparency = 1
            delBtn.Text = "🗑"; delBtn.TextSize = 14; delBtn.BorderSizePixel = 0
            delBtn.ZIndex = 29; delBtn.Parent = row; addCorner(delBtn, 4)
            delBtn.MouseEnter:Connect(function() delBtn.BackgroundTransparency = 0 end)
            delBtn.MouseLeave:Connect(function() delBtn.BackgroundTransparency = 1 end)

            row.MouseEnter:Connect(function() row.BackgroundColor3 = BG_PANEL_HOVER end)
            row.MouseLeave:Connect(function() row.BackgroundColor3 = BG_PANEL_ITEM end)

            local capturedScript = script
            loadBtn.MouseButton1Click:Connect(function()
                createNewTab(capturedScript.name, capturedScript.content)
                closeOpenPanel()
                showToast("Loaded: "..capturedScript.name, ACCENT_BLUE)
            end)

            delBtn.MouseButton1Click:Connect(function()
                local delName = capturedScript.name
                dbDelete(delName)
                row:Destroy()
                -- rebuild canvas
                task.defer(function()
                    scrollFrame.CanvasSize = UDim2.fromOffset(0, listLayout.AbsoluteContentSize.Y + 8)
                end)
                showToast("Deleted: "..delName, ACCENT_RED)
                -- show empty state if nothing left
                if #dbList() == 0 then
                    buildScriptList(scrollFrame, listLayout)
                end
            end)
        end
    end

    task.defer(function()
        scrollFrame.CanvasSize = UDim2.fromOffset(0, listLayout.AbsoluteContentSize.Y + 8)
    end)
end

local function openScriptBrowser()
    if openPanel then closeOpenPanel() end
    saveActiveTabContent()

    openOverlay = makeOverlay(20)
    openOverlay.MouseButton1Click:Connect(closeOpenPanel)

    openPanel = Instance.new("Frame")
    openPanel.Name = "OpenPanel"; openPanel.Size = UDim2.fromOffset(520,400)
    openPanel.Position = UDim2.fromScale(0.5,0.5); openPanel.AnchorPoint = Vector2.new(0.5,0.5)
    openPanel.BackgroundColor3 = BG_PANEL; openPanel.BorderSizePixel = 0
    openPanel.ZIndex = 25; openPanel.Parent = frame
    addCorner(openPanel, 8); addStroke(openPanel, Color3.fromRGB(25,55,38), 1)

    -- Header
    local opTitle = Instance.new("TextLabel")
    opTitle.Size = UDim2.new(1,-16,0,48); opTitle.Position = UDim2.fromOffset(16,0)
    opTitle.BackgroundTransparency = 1; opTitle.Text = "⬒  Script Database"
    opTitle.TextColor3 = TEXT_PRIMARY; opTitle.TextSize = 15; opTitle.Font = Enum.Font.GothamMedium
    opTitle.TextXAlignment = Enum.TextXAlignment.Left; opTitle.ZIndex = 26; opTitle.Parent = openPanel
    hline(openPanel, 48, 26)

    local opClose = Instance.new("TextButton")
    opClose.Size = UDim2.fromOffset(28,28); opClose.Position = UDim2.new(1,-44,0,10)
    opClose.BackgroundColor3 = BG_BUTTON; opClose.Text = "✕"; opClose.TextColor3 = TEXT_DIM
    opClose.TextSize = 13; opClose.Font = Enum.Font.GothamMedium; opClose.BorderSizePixel = 0
    opClose.ZIndex = 27; opClose.Parent = openPanel; addCorner(opClose, 4)
    opClose.MouseButton1Click:Connect(closeOpenPanel)
    opClose.MouseEnter:Connect(function() opClose.TextColor3 = ACCENT_RED end)
    opClose.MouseLeave:Connect(function() opClose.TextColor3 = TEXT_DIM end)

    -- Scrolling list
    local scriptScroll = Instance.new("ScrollingFrame")
    scriptScroll.Size = UDim2.new(1,-16,1,-96); scriptScroll.Position = UDim2.fromOffset(8,56)
    scriptScroll.BackgroundTransparency = 1; scriptScroll.BorderSizePixel = 0
    scriptScroll.ScrollBarThickness = 4; scriptScroll.ScrollBarImageColor3 = Color3.fromRGB(55,55,55)
    scriptScroll.CanvasSize = UDim2.fromOffset(0,0); scriptScroll.ZIndex = 26; scriptScroll.Parent = openPanel

    local listLayout = Instance.new("UIListLayout")
    listLayout.FillDirection = Enum.FillDirection.Vertical; listLayout.SortOrder = Enum.SortOrder.LayoutOrder
    listLayout.Padding = UDim.new(0,4); listLayout.Parent = scriptScroll

    local listPad = Instance.new("UIPadding")
    listPad.PaddingTop = UDim.new(0,4); listPad.PaddingBottom = UDim.new(0,4)
    listPad.PaddingLeft = UDim.new(0,4); listPad.Parent = scriptScroll

    buildScriptList(scriptScroll, listLayout)

    -- Footer
    hline(openPanel, openPanel.Size.Y.Offset - 40, 26)
    local footerNote = Instance.new("TextLabel")
    footerNote.Size = UDim2.new(1,-16,0,38); footerNote.Position = UDim2.new(0,8,1,-40)
    footerNote.BackgroundTransparency = 1
    footerNote.Text = "Scripts persist for this session.  Press  Save  to add a script."
    footerNote.TextColor3 = TEXT_MUTED; footerNote.TextSize = 11; footerNote.Font = Enum.Font.Gotham
    footerNote.ZIndex = 26; footerNote.Parent = openPanel
end

-- ─────────────────────────────────────────────────────────────
-- BUTTON WIRING
-- ─────────────────────────────────────────────────────────────
clearButton.MouseButton1Click:Connect(function()
    codeBox.Text = ""
    if tabs[activeTabIndex] then tabs[activeTabIndex].content = "" end
    showToast("Tab cleared", TEXT_DIM)
end)
openButton.MouseButton1Click:Connect(openScriptBrowser)
saveButton.MouseButton1Click:Connect(openSavePanel)

-- ─────────────────────────────────────────────────────────────
-- DRAGGING
-- ─────────────────────────────────────────────────────────────
local dragging = false
local dragStart, frameStart

local function updateDrag(input)
    if not dragging or input.UserInputType ~= Enum.UserInputType.MouseMovement then return end
    local d = input.Position - dragStart
    frame.Position = UDim2.new(frameStart.X.Scale, frameStart.X.Offset+d.X,
                                frameStart.Y.Scale, frameStart.Y.Offset+d.Y)
end
titleBar.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 then
        dragging = true; dragStart = input.Position; frameStart = frame.Position
    end
end)
titleBar.InputEnded:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 then dragging = false end
end)
UIS.InputChanged:Connect(updateDrag)

-- ─────────────────────────────────────────────────────────────
-- OUTPUT HELPER
-- ─────────────────────────────────────────────────────────────
local function write(...)
    local parts = {}
    for i = 1, select("#",...) do parts[i] = tostring(select(i,...)) end
    outputBox.Text ..= table.concat(parts," ").."\n"
    outputBox.Visible = true
end

-- ─────────────────────────────────────────────────────────────
-- ATTACH
-- ─────────────────────────────────────────────────────────────
local isAttached = false
attachButton.MouseButton1Click:Connect(function()
    if isAttached then return end
    isAttached = true
    statusDot.BackgroundColor3 = ACCENT_ORANGE; signalIcon.TextColor3 = ACCENT_ORANGE
    write("Attach sequence initiated. Waiting 5 seconds...")
    task.spawn(function()
        for _ = 1, 5 do task.wait(1) end
        if isAttached then
            statusDot.BackgroundColor3 = ACCENT_GREEN; signalIcon.TextColor3 = ACCENT_GREEN
            write("Attachment successful. Ready for execution.")
            showToast("Attached successfully", ACCENT_GREEN)
        else
            statusDot.BackgroundColor3 = ACCENT_RED; signalIcon.TextColor3 = ACCENT_RED
            write("Attachment failed.")
        end
    end)
end)

-- ─────────────────────────────────────────────────────────────
-- EXECUTE
-- ─────────────────────────────────────────────────────────────
executeButton.MouseButton1Click:Connect(function()
    if not isAttached or statusDot.BackgroundColor3 ~= ACCENT_GREEN then
        write("Cannot execute: not attached. Click Attach first."); return
    end
    saveActiveTabContent()
    local code = tabs[activeTabIndex] and tabs[activeTabIndex].content or ""
    write("────────────────────────────"); write("Executing...")
    if typeof(loadstring) ~= "function" then write("Error: loadstring unavailable."); return end
    local func, err = loadstring(code)
    if not func then write("Compile Error: "..tostring(err)); return end
    local env = getfenv(func)
    env.print = function(...) write(...) end
    local ok, result = pcall(func)
    if ok then
        write(result ~= nil and ("Returned: "..tostring(result)) or "Execution complete.")
    else
        write("Runtime Error: "..tostring(result))
    end
end)

print("Kernel Executor v4 loaded.")

-- ─────────────────────────────────────────────────────────────
-- STARTUP BADGE  (bottom-right corner, shows on load)
-- ─────────────────────────────────────────────────────────────
local BADGE_W = 220
local BADGE_H = 44

local startupBadge = Instance.new("Frame")
startupBadge.Name             = "StartupBadge"
startupBadge.Size             = UDim2.fromOffset(BADGE_W, BADGE_H)
startupBadge.Position         = UDim2.new(1, BADGE_W + 20, 1, -(BADGE_H + 16 + 36))
startupBadge.BackgroundColor3 = Color3.fromRGB(14, 16, 20)
startupBadge.BorderSizePixel  = 0
startupBadge.ZIndex           = 160
startupBadge.Visible          = false
startupBadge.Parent           = gui
addCorner(startupBadge, 8)
addStroke(startupBadge, Color3.fromRGB(40, 180, 100), 1)

-- Green left glow bar
local badgeBar = Instance.new("Frame")
badgeBar.Size             = UDim2.fromOffset(3, BADGE_H - 14)
badgeBar.Position         = UDim2.fromOffset(9, 7)
badgeBar.BackgroundColor3 = Color3.fromRGB(72, 225, 128)
badgeBar.BorderSizePixel  = 0
badgeBar.ZIndex           = 161
badgeBar.Parent           = startupBadge
addCorner(badgeBar, 2)

-- Pulsing dot
local badgeDot = Instance.new("Frame")
badgeDot.Size             = UDim2.fromOffset(6, 6)
badgeDot.Position         = UDim2.fromOffset(18, 10)
badgeDot.BackgroundColor3 = Color3.fromRGB(72, 225, 128)
badgeDot.BorderSizePixel  = 0
badgeDot.ZIndex           = 161
badgeDot.Parent           = startupBadge
addCorner(badgeDot, 3)

-- Top line: "Kernel"
local badgeTitle = Instance.new("TextLabel")
badgeTitle.Size             = UDim2.fromOffset(BADGE_W - 36, 18)
badgeTitle.Position         = UDim2.fromOffset(30, 6)
badgeTitle.BackgroundTransparency = 1
badgeTitle.Text             = "Kernel  v4"
badgeTitle.TextColor3       = Color3.fromRGB(72, 225, 128)
badgeTitle.TextSize         = 11
badgeTitle.Font             = Enum.Font.GothamBold
badgeTitle.TextXAlignment   = Enum.TextXAlignment.Left
badgeTitle.ZIndex           = 161
badgeTitle.Parent           = startupBadge

-- Bottom line: "Executor loaded"
local badgeMsg = Instance.new("TextLabel")
badgeMsg.Size             = UDim2.fromOffset(BADGE_W - 36, 16)
badgeMsg.Position         = UDim2.fromOffset(30, 24)
badgeMsg.BackgroundTransparency = 1
badgeMsg.Text             = "Executor loaded"
badgeMsg.TextColor3       = Color3.fromRGB(160, 175, 185)
badgeMsg.TextSize         = 10
badgeMsg.Font             = Enum.Font.Gotham
badgeMsg.TextXAlignment   = Enum.TextXAlignment.Left
badgeMsg.ZIndex           = 161
badgeMsg.Parent           = startupBadge

-- Startup sound (different from toast — deeper, satisfying boot chime)
local startupSound = Instance.new("Sound")
startupSound.SoundId  = "rbxassetid://4590662766"
startupSound.Volume   = 0.65
startupSound.Parent   = game:GetService("SoundService")

local BADGE_IN  = UDim2.new(1, -(BADGE_W + 16), 1, -(BADGE_H + 16 + 36))
local BADGE_OUT = UDim2.new(1, BADGE_W + 20,     1, -(BADGE_H + 16 + 36))

local function showStartupBadge()
    startupBadge.Position = BADGE_OUT
    startupBadge.Visible  = true
    startupSound:Play()

    -- Slide in
    TweenSvc:Create(startupBadge,
        TweenInfo.new(0.35, Enum.EasingStyle.Quart, Enum.EasingDirection.Out),
        { Position = BADGE_IN }
    ):Play()

    -- Pulse the dot
    task.spawn(function()
        for _ = 1, 5 do
            TweenSvc:Create(badgeDot, TweenInfo.new(0.4), { BackgroundTransparency = 0.8 }):Play()
            task.wait(0.4)
            TweenSvc:Create(badgeDot, TweenInfo.new(0.4), { BackgroundTransparency = 0 }):Play()
            task.wait(0.4)
        end
    end)

    -- Hold then slide out and destroy
    task.delay(3.2, function()
        TweenSvc:Create(startupBadge,
            TweenInfo.new(0.25, Enum.EasingStyle.Quart, Enum.EasingDirection.In),
            { Position = BADGE_OUT }
        ):Play()
        task.delay(0.3, function()
            startupBadge:Destroy()
            startupSound:Destroy()
        end)
    end)
end

-- Reveal main GUI once loading sequence finishes
waitForReady(function()
    finishLoading(frame)
    task.delay(0.7, function()
        showStartupBadge()
    end)
end)
