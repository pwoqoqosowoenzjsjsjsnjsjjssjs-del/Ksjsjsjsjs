local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UIS = game:GetService("UserInputService")
local TweenService = game:GetService("TweenService")
local TeleportService = game:GetService("TeleportService")
local VirtualUser = game:GetService("VirtualUser")
local player = Players.LocalPlayer

local function char() return player.Character or player.CharacterAdded:Wait() end
local function hum() return char():WaitForChild("Humanoid") end
local function root() return char():WaitForChild("HumanoidRootPart") end

local function tween(o, t, p, s, d)
    local tw = TweenService:Create(o, TweenInfo.new(t, s or Enum.EasingStyle.Quint, d or Enum.EasingDirection.Out), p)
    tw:Play()
    return tw
end
local function corner(p, r) local c = Instance.new("UICorner", p) c.CornerRadius = UDim.new(0, r or 8) return c end
local function strokeOf(p, col, tr) local s = Instance.new("UIStroke", p) s.Color = col s.Transparency = tr or 0 s.Thickness = 1 return s end

local C = {
    bg = Color3.fromRGB(16, 16, 20),
    side = Color3.fromRGB(21, 21, 26),
    card = Color3.fromRGB(27, 27, 33),
    hover = Color3.fromRGB(34, 34, 42),
    off = Color3.fromRGB(52, 52, 62),
    accent = Color3.fromRGB(48, 255, 106),
    text = Color3.fromRGB(240, 240, 245),
    sub = Color3.fromRGB(140, 140, 155),
    line = Color3.fromRGB(38, 38, 46),
}

local gui = Instance.new("ScreenGui")
gui.Name = "PeidonesHubBABFT"
gui.ResetOnSpawn = false
gui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
gui.Parent = (gethui and gethui()) or player:WaitForChild("PlayerGui")

local W, H, SIDE = 540, 350, 150
local main = Instance.new("Frame", gui)
main.Size = UDim2.fromOffset(W, H)
main.Position = UDim2.fromScale(0.5, 0.5)
main.AnchorPoint = Vector2.new(0.5, 0.5)
main.BackgroundColor3 = C.bg
main.BackgroundTransparency = 0.04
main.BorderSizePixel = 0
main.ClipsDescendants = true
corner(main, 14)
strokeOf(main, Color3.fromRGB(255, 255, 255), 0.92)
local scale = Instance.new("UIScale", main)
scale.Scale = 0

local top = Instance.new("Frame", main)
top.Size = UDim2.new(1, 0, 0, 44)
top.BackgroundTransparency = 1

local icon = Instance.new("Frame", top)
icon.Size = UDim2.fromOffset(22, 22)
icon.Position = UDim2.fromOffset(14, 11)
icon.BackgroundColor3 = C.accent
corner(icon, 6)
local iconTxt = Instance.new("TextLabel", icon)
iconTxt.Size = UDim2.fromScale(1, 1)
iconTxt.BackgroundTransparency = 1
iconTxt.Text = "P"
iconTxt.Font = Enum.Font.GothamBold
iconTxt.TextSize = 13
iconTxt.TextColor3 = C.bg

local title = Instance.new("TextLabel", top)
title.Size = UDim2.new(0, 200, 0, 16)
title.Position = UDim2.fromOffset(44, 8)
title.BackgroundTransparency = 1
title.Text = "Peidones Hub"
title.Font = Enum.Font.GothamBold
title.TextSize = 14
title.TextColor3 = C.text
title.TextXAlignment = Enum.TextXAlignment.Left

local subtitle = Instance.new("TextLabel", top)
subtitle.Size = UDim2.new(0, 200, 0, 14)
subtitle.Position = UDim2.fromOffset(44, 24)
subtitle.BackgroundTransparency = 1
subtitle.Text = "Build A Boat For Treasure"
subtitle.Font = Enum.Font.Gotham
subtitle.TextSize = 11
subtitle.TextColor3 = C.sub
subtitle.TextXAlignment = Enum.TextXAlignment.Left

local function topBtn(txt, x)
    local b = Instance.new("TextButton", top)
    b.Size = UDim2.fromOffset(26, 26)
    b.Position = UDim2.new(1, x, 0, 9)
    b.BackgroundColor3 = C.card
    b.BackgroundTransparency = 1
    b.Text = txt
    b.Font = Enum.Font.GothamMedium
    b.TextSize = 14
    b.TextColor3 = C.sub
    b.AutoButtonColor = false
    corner(b, 7)
    b.MouseEnter:Connect(function() tween(b, 0.2, {BackgroundTransparency = 0, TextColor3 = C.text}) end)
    b.MouseLeave:Connect(function() tween(b, 0.2, {BackgroundTransparency = 1, TextColor3 = C.sub}) end)
    return b
end
local closeBtn = topBtn("✕", -36)
local minBtn = topBtn("—", -66)

local topLine = Instance.new("Frame", main)
topLine.Size = UDim2.new(1, 0, 0, 1)
topLine.Position = UDim2.fromOffset(0, 44)
topLine.BackgroundColor3 = C.line
topLine.BorderSizePixel = 0

local side = Instance.new("Frame", main)
side.Size = UDim2.new(0, SIDE, 1, -45)
side.Position = UDim2.fromOffset(0, 45)
side.BackgroundColor3 = C.side
side.BorderSizePixel = 0

local sideLine = Instance.new("Frame", side)
sideLine.Size = UDim2.new(0, 1, 1, 0)
sideLine.Position = UDim2.new(1, 0, 0, 0)
sideLine.BackgroundColor3 = C.line
sideLine.BorderSizePixel = 0

local BTN_H, BTN_PAD, BTN_TOP = 32, 4, 10
local indicator = Instance.new("Frame", side)
indicator.Size = UDim2.new(1, -16, 0, BTN_H)
indicator.Position = UDim2.fromOffset(8, BTN_TOP)
indicator.BackgroundColor3 = C.hover
indicator.BorderSizePixel = 0
corner(indicator, 8)
local bar = Instance.new("Frame", indicator)
bar.Size = UDim2.fromOffset(3, 14)
bar.Position = UDim2.new(0, 0, 0.5, -7)
bar.BackgroundColor3 = C.accent
bar.BorderSizePixel = 0
corner(bar, 2)

local tabList = Instance.new("Frame", side)
tabList.Size = UDim2.new(1, -16, 1, -BTN_TOP)
tabList.Position = UDim2.fromOffset(8, BTN_TOP)
tabList.BackgroundTransparency = 1
local tl = Instance.new("UIListLayout", tabList)
tl.Padding = UDim.new(0, BTN_PAD)
tl.SortOrder = Enum.SortOrder.LayoutOrder

local holder = Instance.new("CanvasGroup", main)
holder.Size = UDim2.new(1, -SIDE - 1, 1, -45)
holder.Position = UDim2.fromOffset(SIDE + 1, 45)
holder.BackgroundTransparency = 1
holder.BorderSizePixel = 0

local notifs = Instance.new("Frame", gui)
notifs.Size = UDim2.fromOffset(260, 400)
notifs.Position = UDim2.new(1, -20, 1, -20)
notifs.AnchorPoint = Vector2.new(1, 1)
notifs.BackgroundTransparency = 1
local nl = Instance.new("UIListLayout", notifs)
nl.Padding = UDim.new(0, 8)
nl.VerticalAlignment = Enum.VerticalAlignment.Bottom
nl.SortOrder = Enum.SortOrder.LayoutOrder

local function notify(t, msg, dur)
    local wrap = Instance.new("Frame", notifs)
    wrap.Size = UDim2.new(1, 0, 0, 58)
    wrap.BackgroundTransparency = 1
    local card = Instance.new("Frame", wrap)
    card.Size = UDim2.fromScale(1, 1)
    card.Position = UDim2.new(1, 30, 0, 0)
    card.BackgroundColor3 = C.bg
    card.BorderSizePixel = 0
    corner(card, 10)
    strokeOf(card, Color3.new(1, 1, 1), 0.9)
    local acc = Instance.new("Frame", card)
    acc.Size = UDim2.new(0, 3, 1, -20)
    acc.Position = UDim2.fromOffset(10, 10)
    acc.BackgroundColor3 = C.accent
    acc.BorderSizePixel = 0
    corner(acc, 2)
    local a = Instance.new("TextLabel", card)
    a.Size = UDim2.new(1, -30, 0, 18)
    a.Position = UDim2.fromOffset(22, 10)
    a.BackgroundTransparency = 1
    a.Text = t
    a.Font = Enum.Font.GothamBold
    a.TextSize = 13
    a.TextColor3 = C.text
    a.TextXAlignment = Enum.TextXAlignment.Left
    local b = Instance.new("TextLabel", card)
    b.Size = UDim2.new(1, -30, 0, 16)
    b.Position = UDim2.fromOffset(22, 30)
    b.BackgroundTransparency = 1
    b.Text = msg
    b.Font = Enum.Font.Gotham
    b.TextSize = 12
    b.TextColor3 = C.sub
    b.TextXAlignment = Enum.TextXAlignment.Left
    b.TextTruncate = Enum.TextTruncate.AtEnd
    tween(card, 0.5, {Position = UDim2.new()}, Enum.EasingStyle.Back)
    task.delay(dur or 3, function()
        tween(card, 0.35, {Position = UDim2.new(1, 30, 0, 0)}, Enum.EasingStyle.Quint, Enum.EasingDirection.In).Completed:Wait()
        tween(wrap, 0.2, {Size = UDim2.new(1, 0, 0, 0)}).Completed:Wait()
        wrap:Destroy()
    end)
end

tween(scale, 0.5, {Scale = 1}, Enum.EasingStyle.Back)

local visible, minimized = true, false
local function setVisible(v)
    visible = v
    if v then
        main.Visible = true
        tween(scale, 0.4, {Scale = 1}, Enum.EasingStyle.Back)
    else
        tween(scale, 0.25, {Scale = 0}, Enum.EasingStyle.Quint, Enum.EasingDirection.In).Completed:Connect(function()
            if not visible then main.Visible = false end
        end)
    end
end

closeBtn.MouseButton1Click:Connect(function()
    tween(scale, 0.25, {Scale = 0}, Enum.EasingStyle.Quint, Enum.EasingDirection.In).Completed:Connect(function() gui:Destroy() end)
end)
minBtn.MouseButton1Click:Connect(function()
    minimized = not minimized
    tween(main, 0.4, {Size = UDim2.fromOffset(minimized and 260 or W, minimized and 44 or H)})
end)
UIS.InputBegan:Connect(function(i, gp)
    if not gp and i.KeyCode == Enum.KeyCode.RightControl then setVisible(not visible) end
end)

do
    local dragging, sp, sm
    top.InputBegan:Connect(function(i)
        if i.UserInputType == Enum.UserInputType.MouseButton1 or i.UserInputType == Enum.UserInputType.Touch then
            dragging, sp, sm = true, main.Position, i.Position
            i.Changed:Connect(function() if i.UserInputState == Enum.UserInputState.End then dragging = false end end)
        end
    end)
    UIS.InputChanged:Connect(function(i)
        if dragging and (i.UserInputType == Enum.UserInputType.MouseMovement or i.UserInputType == Enum.UserInputType.Touch) then
            local d = i.Position - sm
            tween(main, 0.14, {Position = UDim2.new(sp.X.Scale, sp.X.Offset + d.X, sp.Y.Scale, sp.Y.Offset + d.Y)})
        end
    end)
end

local tabs, current = {}, nil

local function selectTab(t)
    if current == t then return end
    for _, o in ipairs(tabs) do
        o.page.Visible = false
        tween(o.btn, 0.2, {TextColor3 = C.sub})
    end
    current = t
    tween(indicator, 0.35, {Position = UDim2.fromOffset(8, BTN_TOP + (t.index - 1) * (BTN_H + BTN_PAD))}, Enum.EasingStyle.Quint)
    tween(t.btn, 0.2, {TextColor3 = C.text})
    t.page.Visible = true
    t.page.Position = UDim2.fromOffset(14, 26)
    holder.GroupTransparency = 1
    tween(t.page, 0.35, {Position = UDim2.fromOffset(14, 12)})
    tween(holder, 0.3, {GroupTransparency = 0})
end

local function Tab(name, glyph)
    local t = {index = #tabs + 1}
    local btn = Instance.new("TextButton", tabList)
    btn.Size = UDim2.new(1, 0, 0, BTN_H)
    btn.BackgroundTransparency = 1
    btn.Text = "   " .. glyph .. "   " .. name
    btn.Font = Enum.Font.GothamMedium
    btn.TextSize = 13
    btn.TextColor3 = C.sub
    btn.TextXAlignment = Enum.TextXAlignment.Left
    btn.LayoutOrder = t.index
    btn.AutoButtonColor = false
    btn.MouseEnter:Connect(function() if current ~= t then tween(btn, 0.15, {TextColor3 = C.text}) end end)
    btn.MouseLeave:Connect(function() if current ~= t then tween(btn, 0.15, {TextColor3 = C.sub}) end end)

    local page = Instance.new("ScrollingFrame", holder)
    page.Size = UDim2.new(1, -28, 1, -24)
    page.Position = UDim2.fromOffset(14, 12)
    page.BackgroundTransparency = 1
    page.BorderSizePixel = 0
    page.ScrollBarThickness = 2
    page.ScrollBarImageColor3 = C.off
    page.AutomaticCanvasSize = Enum.AutomaticSize.Y
    page.CanvasSize = UDim2.new()
    page.Visible = false
    local pl = Instance.new("UIListLayout", page)
    pl.Padding = UDim.new(0, 6)
    pl.SortOrder = Enum.SortOrder.LayoutOrder
    Instance.new("UIPadding", page).PaddingBottom = UDim.new(0, 8)

    t.btn, t.page, t.order = btn, page, 0
    btn.MouseButton1Click:Connect(function() selectTab(t) end)
    table.insert(tabs, t)
    if t.index == 1 then
        current = t
        page.Visible = true
        btn.TextColor3 = C.text
    end

    local function card(h)
        t.order += 1
        local f = Instance.new("Frame", page)
        f.Size = UDim2.new(1, -4, 0, h)
        f.BackgroundColor3 = C.card
        f.BorderSizePixel = 0
        f.LayoutOrder = t.order
        corner(f, 9)
        f.MouseEnter:Connect(function() tween(f, 0.2, {BackgroundColor3 = C.hover}) end)
        f.MouseLeave:Connect(function() tween(f, 0.2, {BackgroundColor3 = C.card}) end)
        return f
    end
    local function lbl(p, txt, desc)
        local l = Instance.new("TextLabel", p)
        l.BackgroundTransparency = 1
        l.Text = txt
        l.Font = Enum.Font.GothamMedium
        l.TextSize = 13
        l.TextColor3 = C.text
        l.TextXAlignment = Enum.TextXAlignment.Left
        if desc then
            l.Size = UDim2.new(1, -80, 0, 16)
            l.Position = UDim2.fromOffset(14, 8)
            local d = Instance.new("TextLabel", p)
            d.Size = UDim2.new(1, -80, 0, 14)
            d.Position = UDim2.fromOffset(14, 25)
            d.BackgroundTransparency = 1
            d.Text = desc
            d.Font = Enum.Font.Gotham
            d.TextSize = 11
            d.TextColor3 = C.sub
            d.TextXAlignment = Enum.TextXAlignment.Left
        else
            l.Size = UDim2.new(1, -80, 1, 0)
            l.Position = UDim2.fromOffset(14, 0)
        end
        return l
    end

    function t:Section(txt)
        t.order += 1
        local l = Instance.new("TextLabel", page)
        l.Size = UDim2.new(1, 0, 0, 22)
        l.BackgroundTransparency = 1
        l.Text = string.upper(txt)
        l.Font = Enum.Font.GothamBold
        l.TextSize = 10
        l.TextColor3 = C.sub
        l.TextXAlignment = Enum.TextXAlignment.Left
        l.LayoutOrder = t.order
    end

    function t:Toggle(txt, desc, cb)
        local f = card(desc and 48 or 36)
        lbl(f, txt, desc)
        local sw = Instance.new("Frame", f)
        sw.Size = UDim2.fromOffset(38, 20)
        sw.Position = UDim2.new(1, -52, 0.5, -10)
        sw.BackgroundColor3 = C.off
        corner(sw, 10)
        local k = Instance.new("Frame", sw)
        k.Size = UDim2.fromOffset(16, 16)
        k.Position = UDim2.fromOffset(2, 2)
        k.BackgroundColor3 = Color3.new(1, 1, 1)
        corner(k, 8)
        local hit = Instance.new("TextButton", f)
        hit.Size = UDim2.fromScale(1, 1)
        hit.BackgroundTransparency = 1
        hit.Text = ""
        local on = false
        hit.MouseButton1Click:Connect(function()
            on = not on
            tween(sw, 0.25, {BackgroundColor3 = on and C.accent or C.off})
            tween(k, 0.35, {Position = UDim2.fromOffset(on and 20 or 2, 2), BackgroundColor3 = on and C.bg or Color3.new(1, 1, 1)}, Enum.EasingStyle.Back)
            task.spawn(cb, on)
        end)
    end

    function t:Button(txt, desc, cb)
        local f = card(desc and 48 or 36)
        lbl(f, txt, desc)
        local arrow = Instance.new("TextLabel", f)
        arrow.Size = UDim2.fromOffset(20, 20)
        arrow.Position = UDim2.new(1, -34, 0.5, -10)
        arrow.BackgroundTransparency = 1
        arrow.Text = "›"
        arrow.Font = Enum.Font.GothamBold
        arrow.TextSize = 18
        arrow.TextColor3 = C.sub
        local hit = Instance.new("TextButton", f)
        hit.Size = UDim2.fromScale(1, 1)
        hit.BackgroundTransparency = 1
        hit.Text = ""
        hit.MouseButton1Click:Connect(function()
            tween(arrow, 0.12, {Position = UDim2.new(1, -28, 0.5, -10), TextColor3 = C.accent}).Completed:Connect(function()
                tween(arrow, 0.25, {Position = UDim2.new(1, -34, 0.5, -10), TextColor3 = C.sub}, Enum.EasingStyle.Back)
            end)
            task.spawn(cb)
        end)
    end

    function t:Slider(txt, min, max, def, cb)
        local f = card(52)
        local l = lbl(f, txt)
        l.Size = UDim2.new(1, -80, 0, 30)
        local val = Instance.new("TextLabel", f)
        val.Size = UDim2.fromOffset(60, 30)
        val.Position = UDim2.new(1, -74, 0, 0)
        val.BackgroundTransparency = 1
        val.Text = tostring(def)
        val.Font = Enum.Font.GothamMedium
        val.TextSize = 12
        val.TextColor3 = C.sub
        val.TextXAlignment = Enum.TextXAlignment.Right
        local track = Instance.new("Frame", f)
        track.Size = UDim2.new(1, -28, 0, 5)
        track.Position = UDim2.fromOffset(14, 34)
        track.BackgroundColor3 = C.off
        track.BorderSizePixel = 0
        corner(track, 3)
        local a0 = (def - min) / (max - min)
        local fill = Instance.new("Frame", track)
        fill.Size = UDim2.fromScale(a0, 1)
        fill.BackgroundColor3 = C.accent
        fill.BorderSizePixel = 0
        corner(fill, 3)
        local k = Instance.new("Frame", track)
        k.Size = UDim2.fromOffset(13, 13)
        k.AnchorPoint = Vector2.new(0.5, 0.5)
        k.Position = UDim2.fromScale(a0, 0.5)
        k.BackgroundColor3 = Color3.new(1, 1, 1)
        corner(k, 7)
        local dragging = false
        local function set(x)
            local a = math.clamp((x - track.AbsolutePosition.X) / track.AbsoluteSize.X, 0, 1)
            local v = math.floor(min + (max - min) * a + 0.5)
            val.Text = tostring(v)
            tween(fill, 0.12, {Size = UDim2.fromScale(a, 1)})
            tween(k, 0.12, {Position = UDim2.fromScale(a, 0.5)})
            cb(v)
        end
        local hit = Instance.new("TextButton", f)
        hit.Size = UDim2.new(1, 0, 0, 24)
        hit.Position = UDim2.fromOffset(0, 26)
        hit.BackgroundTransparency = 1
        hit.Text = ""
        hit.InputBegan:Connect(function(i)
            if i.UserInputType == Enum.UserInputType.MouseButton1 or i.UserInputType == Enum.UserInputType.Touch then
                dragging = true
                tween(k, 0.15, {Size = UDim2.fromOffset(17, 17)})
                tween(val, 0.15, {TextColor3 = C.accent})
                set(i.Position.X)
            end
        end)
        UIS.InputChanged:Connect(function(i)
            if dragging and (i.UserInputType == Enum.UserInputType.MouseMovement or i.UserInputType == Enum.UserInputType.Touch) then set(i.Position.X) end
        end)
        UIS.InputEnded:Connect(function(i)
            if dragging and (i.UserInputType == Enum.UserInputType.MouseButton1 or i.UserInputType == Enum.UserInputType.Touch) then
                dragging = false
                tween(k, 0.15, {Size = UDim2.fromOffset(13, 13)})
                tween(val, 0.15, {TextColor3 = C.sub})
            end
        end)
    end

    return t
end

local function stages()
    local bs = workspace:FindFirstChild("BoatStages")
    return bs and bs:FindFirstChild("NormalStages")
end

local function touch(part)
    local r = root()
    if firetouchinterest then
        firetouchinterest(r, part, 0)
        task.wait()
        firetouchinterest(r, part, 1)
    end
end

local farming, farmDelay, runs = false, 2, 0

local function goToChest()
    local s = stages()
    local e = s and s:FindFirstChild("TheEnd")
    local chest = e and e:FindFirstChild("GoldenChest")
    local trig = chest and chest:FindFirstChild("Trigger")
    if trig then
        root().CFrame = trig.CFrame
        touch(trig)
        return true
    end
    return false
end

local function farmRun()
    local s = stages()
    if not s then notify("Erro", "Estágios não encontrados") return false end
    for i = 1, 10 do
        if not farming then return false end
        local st = s:FindFirstChild("CaveStage" .. i)
        local dp = st and st:FindFirstChild("DarknessPart")
        if dp then
            local r = root()
            r.CFrame = dp.CFrame
            r.AssemblyLinearVelocity = Vector3.zero
            task.wait(farmDelay)
        end
    end
    if not farming then return false end
    return goToChest()
end

task.spawn(function()
    while true do
        if farming then
            local ok = farmRun()
            if ok then
                runs += 1
                notify("Auto Farm", "Run " .. runs .. " concluída, esperando respawn")
                local c = player.CharacterAdded:Wait()
                c:WaitForChild("HumanoidRootPart", 10)
                task.wait(3)
            else
                task.wait(1)
            end
        else
            task.wait(0.3)
        end
    end
end)

local farm = Tab("Farm", "◆")
farm:Section("Ouro")
farm:Toggle("Auto Farm", "Passa pelos 10 estágios e pega o baú", function(s)
    farming = s
    notify("Auto Farm", s and "Ligado" or "Desligado")
end)
farm:Slider("Delay por estágio (s)", 1, 6, 2, function(v) farmDelay = v end)
farm:Button("Ir pro baú", "Teleporta direto pro final", function()
    if not goToChest() then notify("Erro", "Baú não encontrado") end
end)

local pl = Tab("Player", "●")
pl:Section("Movimento")
pl:Slider("WalkSpeed", 16, 150, 16, function(v) hum().WalkSpeed = v end)
pl:Slider("JumpPower", 50, 250, 50, function(v) local h = hum() h.UseJumpPower = true h.JumpPower = v end)

local infJump = false
pl:Toggle("Pulo infinito", nil, function(s) infJump = s end)
UIS.JumpRequest:Connect(function() if infJump then hum():ChangeState(Enum.HumanoidStateType.Jumping) end end)

local noclip = false
pl:Toggle("Noclip", nil, function(s) noclip = s end)
RunService.Stepped:Connect(function()
    if noclip and player.Character then
        for _, p in ipairs(player.Character:GetDescendants()) do
            if p:IsA("BasePart") then p.CanCollide = false end
        end
    end
end)

local flying, bv, bg, flySpeed = false, nil, nil, 60
pl:Toggle("Fly", "WASD + Espaço / Shift", function(s)
    flying = s
    if s then
        local r = root()
        bv = Instance.new("BodyVelocity", r) bv.MaxForce = Vector3.one * 1e9 bv.Velocity = Vector3.zero
        bg = Instance.new("BodyGyro", r) bg.MaxTorque = Vector3.one * 1e9 bg.P = 9e4
        hum().PlatformStand = true
    else
        if bv then bv:Destroy() bv = nil end
        if bg then bg:Destroy() bg = nil end
        hum().PlatformStand = false
    end
end)
pl:Slider("Velocidade do fly", 10, 200, 60, function(v) flySpeed = v end)
RunService.RenderStepped:Connect(function()
    if not (flying and bv and bg and bv.Parent) then return end
    local cam = workspace.CurrentCamera
    local d = Vector3.zero
    if UIS:IsKeyDown(Enum.KeyCode.W) then d += cam.CFrame.LookVector end
    if UIS:IsKeyDown(Enum.KeyCode.S) then d -= cam.CFrame.LookVector end
    if UIS:IsKeyDown(Enum.KeyCode.A) then d -= cam.CFrame.RightVector end
    if UIS:IsKeyDown(Enum.KeyCode.D) then d += cam.CFrame.RightVector end
    if UIS:IsKeyDown(Enum.KeyCode.Space) then d += Vector3.yAxis end
    if UIS:IsKeyDown(Enum.KeyCode.LeftShift) then d -= Vector3.yAxis end
    bv.Velocity = bv.Velocity:Lerp(d.Magnitude > 0 and d.Unit * flySpeed or Vector3.zero, 0.2)
    bg.CFrame = cam.CFrame
end)

local misc = Tab("Misc", "■")
misc:Section("Utilidades")
local antiAfk
misc:Toggle("Anti-AFK", "Não ser kickado por inatividade", function(s)
    if s then
        antiAfk = player.Idled:Connect(function()
            VirtualUser:CaptureController()
            VirtualUser:ClickButton2(Vector2.new())
        end)
    elseif antiAfk then
        antiAfk:Disconnect()
        antiAfk = nil
    end
end)
misc:Button("Resetar personagem", nil, function() hum().Health = 0 end)
misc:Button("Rejoin", "Entra de novo no servidor", function() TeleportService:Teleport(game.PlaceId, player) end)

task.delay(0.6, function() notify("Peidones Hub", "Carregado · RightControl esconde") end)
