--[[
    ════════════════════════════════════════════════════
     FEATHER HUB  —  IRC Chat System
     Load this separately from Key System
    ════════════════════════════════════════════════════
]]

return function(sg)
    -- Services
    local TweenService = game:GetService("TweenService")
    local UserInputService = game:GetService("UserInputService")
    local HttpService = game:GetService("HttpService")
    local Players = game:GetService("Players")
    local RunService = game:GetService("RunService")
    local Stats = game:GetService("Stats")
    
    local Player = Players.LocalPlayer
    local DisplayName = tostring(Player.DisplayName)
    local Username = tostring(Player.Name)
    
    -- Device detection
    local isMobile = UserInputService.TouchEnabled and not UserInputService.KeyboardEnabled
    
    -- ==========================================
    -- IRC CONFIGURATION
    -- ==========================================
    local IRC_ACCESS_KEY = "$2a$10$haU6YIqH8JwrPqhwOGTw5OkdstOsbeum.DvTUrOx5Lgs/DDh9R/Cy"
    local IRC_BIN_ID = "69b0b8ee864efc355b5ef988"
    local IRC_POLL_RATE = 4
    local IRC_MAX_MSGS = 20
    local IRC_BASE_URL = "https://api.jsonbin.io/v3/b/"
    
    local DISCORD_INVITE = "https://discord.gg/7P8Jaa9fp8"
    
    -- ==========================================
    -- COLOR PALETTE (same as key system)
    -- ==========================================
    local DARK = {
        bg = Color3.fromRGB(14,14,16), sidebar = Color3.fromRGB(20,20,24),
        card = Color3.fromRGB(26,26,30), border = Color3.fromRGB(44,44,52),
        text = Color3.fromRGB(240,240,244), muted = Color3.fromRGB(100,100,115),
        dim = Color3.fromRGB(32,32,38), sep = Color3.fromRGB(36,36,44),
        header = Color3.fromRGB(18,18,22), inputBg = Color3.fromRGB(10,10,14),
        msgSelf = Color3.fromRGB(0,40,80), msgOther = Color3.fromRGB(22,22,28)
    }
    local LIGHT = {
        bg = Color3.fromRGB(248,248,250), sidebar = Color3.fromRGB(255,255,255),
        card = Color3.fromRGB(255,255,255), border = Color3.fromRGB(218,218,228),
        text = Color3.fromRGB(18,18,22), muted = Color3.fromRGB(145,145,160),
        dim = Color3.fromRGB(235,235,242), sep = Color3.fromRGB(225,225,232),
        header = Color3.fromRGB(252,252,255), inputBg = Color3.fromRGB(242,242,248),
        msgSelf = Color3.fromRGB(218,230,255), msgOther = Color3.fromRGB(242,242,248)
    }
    
    local accent = Color3.fromRGB(90,90,210)
    local blue = Color3.fromRGB(50,150,255)
    local green = Color3.fromRGB(40,200,130)
    local red = Color3.fromRGB(240,80,100)
    local gold = Color3.fromRGB(241,196,15)
    local orange = Color3.fromRGB(255,140,0)
    local discordColor = Color3.fromRGB(114,137,218)
    
    local isDark = false
    local T = LIGHT
    
    local function tw(t,s,d) 
        return TweenInfo.new(t or 0.2, s or Enum.EasingStyle.Quart, d or Enum.EasingDirection.Out) 
    end
    
    -- Theme registry
    local themeReg = {}
    local function reg(obj, prop, lv, dv)
        table.insert(themeReg, {obj=obj, prop=prop, l=lv, d=dv})
    end
    
    local function applyThemeReg()
        for _, e in ipairs(themeReg) do
            if e.obj and e.obj.Parent then
                e.obj[e.prop] = isDark and e.d or e.l
            end
        end
    end
    
    -- ==========================================
    -- USERNAME COLORS
    -- ==========================================
    local USER_COLORS = {
        Color3.fromRGB(99,179,237), Color3.fromRGB(154,230,180), Color3.fromRGB(252,129,74),
        Color3.fromRGB(246,173,85), Color3.fromRGB(183,148,246), Color3.fromRGB(246,114,128),
        Color3.fromRGB(79,209,197), Color3.fromRGB(237,137,54),  Color3.fromRGB(129,230,217),
        Color3.fromRGB(214,158,46), Color3.fromRGB(160,174,192), Color3.fromRGB(252,182,159),
    }
    local colorCache = {}
    local function getUserColor(name)
        if colorCache[name] then return colorCache[name] end
        local hash = 0
        for i=1,#name do hash = (hash*31 + string.byte(name,i)) % #USER_COLORS end
        local col = USER_COLORS[(hash % #USER_COLORS)+1]
        colorCache[name]=col
        return col
    end
    
    -- ==========================================
    -- WINDOW SIZING
    -- ==========================================
    local WIN_W, WIN_H, SIDE_W
    if isMobile then
        WIN_W, WIN_H, SIDE_W = 280, 450, 70
    else
        WIN_W, WIN_H, SIDE_W = 440, 480, 120
    end
    
    local IRC_POS
    if isMobile then
        IRC_POS = UDim2.new(0.5, -WIN_W/2, 0.7, 10)
    else
        IRC_POS = UDim2.new(0.98, -WIN_W, 0.5, -WIN_H/2)
    end
    
    -- ==========================================
    -- FPS UNLOCK (shared state with key system)
    -- ==========================================
    local fpsUnlocked = false
    
    local function setFPSCap(value)
        local success = false
        pcall(function() if setfpscap then setfpscap(value) success = true end end)
        if not success then pcall(function() if syn and syn.set_fps_cap then syn.set_fps_cap(value) success = true end end) end
        if not success then pcall(function() if krnl and krnl.set_fpscap then krnl.set_fpscap(value) success = true end end) end
        if not success then pcall(function() if fluxus and fluxus.set_fps_cap then fluxus.set_fps_cap(value) success = true end end) end
        return success
    end
    
    local function toggleFPSUnlock()
        if fpsUnlocked then
            local success = setFPSCap(60)
            if success then
                fpsUnlocked = false
                notify("FPS Unlock", "FPS cap restored to 60", "warning")
            end
        else
            local success = setFPSCap(999)
            if success then
                fpsUnlocked = true
                notify("FPS Unlock", "FPS cap removed!", "success")
            else
                notify("FPS Unlock", "Your executor doesn't support FPS control", "error")
            end
        end
    end
    
    -- ==========================================
    -- COMPONENT BUILDERS (simplified versions)
    -- ==========================================
    local function makeWin(pos)
        local win = Instance.new("Frame")
        win.Size = UDim2.new(0,WIN_W,0,WIN_H)
        win.Position = pos
        win.BackgroundColor3 = T.bg
        win.BorderSizePixel = 0
        win.ClipsDescendants = true
        win.ZIndex = 1
        win.Parent = sg
        Instance.new("UICorner", win).CornerRadius = UDim.new(0,14)
        
        local stroke = Instance.new("UIStroke")
        stroke.Color = T.border
        stroke.Thickness = 1
        stroke.Parent = win
        reg(win,"BackgroundColor3",LIGHT.bg,DARK.bg)
        reg(stroke,"Color",LIGHT.border,DARK.border)
        return win, stroke
    end
    
    local function makeIRCSidebar(win, tabs, onTab)
        local sb = Instance.new("Frame")
        sb.Size = UDim2.new(0,SIDE_W,1,0)
        sb.BackgroundColor3 = T.sidebar
        sb.BorderSizePixel = 0
        sb.ZIndex = 3
        sb.Parent = win
        Instance.new("UICorner", sb).CornerRadius = UDim.new(0,14)
        reg(sb,"BackgroundColor3",LIGHT.sidebar,DARK.sidebar)
        
        -- F logo
        local pill = Instance.new("Frame")
        pill.Size = UDim2.new(0,32,0,32)
        pill.Position = UDim2.new(0.5,-16,0,10)
        pill.BackgroundColor3 = accent
        pill.BorderSizePixel = 0
        pill.ZIndex = 5
        pill.Parent = sb
        Instance.new("UICorner", pill).CornerRadius = UDim.new(0,9)
        
        local logoF = Instance.new("TextLabel")
        logoF.Size = UDim2.new(1,0,1,0)
        logoF.Text = "F"
        logoF.TextSize = 17
        logoF.Font = Enum.Font.GothamBold
        logoF.TextColor3 = Color3.new(1,1,1)
        logoF.BackgroundTransparency = 1
        logoF.ZIndex = 6
        logoF.Parent = pill
        
        -- Navigation
        local nav = Instance.new("Frame")
        nav.Size = UDim2.new(1,0,0,0)
        nav.Position = UDim2.new(0,0,0,56)
        nav.BackgroundTransparency = 1
        nav.ZIndex = 4
        nav.Parent = sb
        
        local navLayout = Instance.new("UIListLayout")
        navLayout.SortOrder = Enum.SortOrder.LayoutOrder
        navLayout.VerticalAlignment = Enum.VerticalAlignment.Top
        navLayout.HorizontalAlignment = Enum.HorizontalAlignment.Center
        navLayout.Padding = UDim.new(0,3)
        navLayout.Parent = nav
        
        local navBtns = {}
        for i, def in ipairs(tabs) do
            local row = Instance.new("Frame")
            row.Size = UDim2.new(1,-6,0,isMobile and 34 or 32)
            row.BackgroundTransparency = 1
            row.LayoutOrder = i
            row.ZIndex = 4
            row.Parent = nav
            
            local bar = Instance.new("Frame")
            bar.Size = UDim2.new(0,3,0.5,0)
            bar.Position = UDim2.new(0,0,0.25,0)
            bar.BackgroundColor3 = def.color
            bar.BackgroundTransparency = (i==1) and 0 or 1
            bar.BorderSizePixel = 0
            bar.ZIndex = 5
            bar.Parent = row
            Instance.new("UICorner", bar).CornerRadius = UDim.new(1,0)
            
            local bg = Instance.new("Frame")
            bg.Size = UDim2.new(1,-4,1,-4)
            bg.Position = UDim2.new(0,4,0,2)
            bg.BackgroundColor3 = (i==1) and def.color or T.dim
            bg.BackgroundTransparency = (i==1) and 0.15 or 0
            bg.BorderSizePixel = 0
            bg.ZIndex = 4
            bg.Parent = row
            Instance.new("UICorner", bg).CornerRadius = UDim.new(0,7)
            
            local ico = Instance.new("TextLabel")
            ico.Size = UDim2.new(0,22,1,0)
            ico.Position = UDim2.new(0,6,0,0)
            ico.Text = def.icon
            ico.TextSize = isMobile and 13 or 12
            ico.Font = Enum.Font.GothamBold
            ico.BackgroundTransparency = 1
            ico.TextTransparency = (i==1) and 0 or 0.35
            ico.ZIndex = 5
            ico.Parent = row
            
            local lbl = Instance.new("TextLabel")
            lbl.Size = UDim2.new(1,-30,1,0)
            lbl.Position = UDim2.new(0,28,0,0)
            lbl.Text = def.name
            lbl.Font = Enum.Font.GothamBold
            lbl.TextSize = isMobile and 8 or 7
            lbl.TextColor3 = (i==1) and def.color or T.muted
            lbl.BackgroundTransparency = 1
            lbl.TextXAlignment = Enum.TextXAlignment.Left
            lbl.ZIndex = 5
            lbl.Parent = row
            
            local hit = Instance.new("TextButton")
            hit.Size = UDim2.new(1,0,1,0)
            hit.BackgroundTransparency = 1
            hit.Text = ""
            hit.ZIndex = 6
            hit.Parent = row
            hit.MouseButton1Click:Connect(function() onTab(i) end)
            hit.TouchTap:Connect(function() onTab(i) end)
            navBtns[i] = {bg=bg, lbl=lbl, ico=ico, bar=bar, color=def.color}
        end
        
        return sb, navBtns
    end
    
    local function makeHeader(win, titleTxt)
        local hdr = Instance.new("Frame")
        hdr.Size = UDim2.new(1,-SIDE_W,0,42)
        hdr.Position = UDim2.new(0,SIDE_W,0,0)
        hdr.BackgroundColor3 = T.header
        hdr.BorderSizePixel = 0
        hdr.ZIndex = 3
        hdr.Parent = win
        Instance.new("UICorner", hdr).CornerRadius = UDim.new(0,14)
        reg(hdr,"BackgroundColor3",LIGHT.header,DARK.header)
        
        local title = Instance.new("TextLabel")
        title.Size = UDim2.new(1,-90,1,0)
        title.Position = UDim2.new(0,14,0,0)
        title.Text = titleTxt
        title.Font = Enum.Font.GothamBold
        title.TextSize = isMobile and 13 or 12
        title.TextColor3 = T.text
        title.BackgroundTransparency = 1
        title.TextXAlignment = Enum.TextXAlignment.Left
        title.ZIndex = 5
        title.Parent = hdr
        reg(title,"TextColor3",LIGHT.text,DARK.text)
        
        -- BETA pill
        local betaPill = Instance.new("Frame")
        betaPill.Size = UDim2.new(0,44,0,16)
        betaPill.Position = UDim2.new(1,-116,0.5,-8)
        betaPill.BackgroundColor3 = isDark and Color3.fromRGB(30,30,44) or Color3.fromRGB(235,235,248)
        betaPill.BorderSizePixel = 0
        betaPill.ZIndex = 5
        betaPill.Parent = hdr
        Instance.new("UICorner", betaPill).CornerRadius = UDim.new(1,0)
        
        local betaTxt = Instance.new("TextLabel")
        betaTxt.Size = UDim2.new(1,0,1,0)
        betaTxt.Text = "BETA"
        betaTxt.Font = Enum.Font.GothamBold
        betaTxt.TextSize = 7
        betaTxt.TextColor3 = gold
        betaTxt.BackgroundTransparency = 1
        betaTxt.ZIndex = 6
        betaTxt.Parent = betaPill
        
        -- Minimize Button
        local minBtn = Instance.new("TextButton")
        minBtn.Size = UDim2.new(0,24,0,24)
        minBtn.Position = UDim2.new(1,-60,0.5,-12)
        minBtn.Text = "−"
        minBtn.Font = Enum.Font.GothamBold
        minBtn.TextSize = 18
        minBtn.TextColor3 = T.muted
        minBtn.BackgroundColor3 = T.dim
        minBtn.BorderSizePixel = 0
        minBtn.AutoButtonColor = false
        minBtn.ZIndex = 5
        minBtn.Parent = hdr
        Instance.new("UICorner", minBtn).CornerRadius = UDim.new(0,6)
        reg(minBtn,"BackgroundColor3",LIGHT.dim,DARK.dim)
        reg(minBtn,"TextColor3",LIGHT.muted,DARK.muted)
        
        -- Close Button
        local closeBtn = Instance.new("TextButton")
        closeBtn.Size = UDim2.new(0,24,0,24)
        closeBtn.Position = UDim2.new(1,-30,0.5,-12)
        closeBtn.Text = "×"
        closeBtn.Font = Enum.Font.GothamBold
        closeBtn.TextSize = 18
        closeBtn.TextColor3 = T.muted
        closeBtn.BackgroundColor3 = T.dim
        closeBtn.BorderSizePixel = 0
        closeBtn.AutoButtonColor = false
        closeBtn.ZIndex = 5
        closeBtn.Parent = hdr
        Instance.new("UICorner", closeBtn).CornerRadius = UDim.new(0,6)
        reg(closeBtn,"BackgroundColor3",LIGHT.dim,DARK.dim)
        reg(closeBtn,"TextColor3",LIGHT.muted,DARK.muted)
        
        return hdr, title, closeBtn, minBtn
    end
    
    local function makeContent(win)
        local c = Instance.new("Frame")
        c.Size = UDim2.new(1,-SIDE_W,1,-42)
        c.Position = UDim2.new(0,SIDE_W,0,42)
        c.BackgroundTransparency = 1
        c.BorderSizePixel = 0
        c.ZIndex = 2
        c.Parent = win
        return c
    end
    
    local function makeDrag(win, hdr)
        local dragging, dragInput, dragStart, startPos
        hdr.InputBegan:Connect(function(i)
            if i.UserInputType == Enum.UserInputType.MouseButton1 or i.UserInputType == Enum.UserInputType.Touch then
                dragging = true
                dragStart = i.Position
                startPos = win.Position
            end
        end)
        hdr.InputChanged:Connect(function(i)
            if i.UserInputType == Enum.UserInputType.MouseMovement or i.UserInputType == Enum.UserInputType.Touch then
                dragInput = i
            end
        end)
        UserInputService.InputChanged:Connect(function(i)
            if i == dragInput and dragging then
                local d = i.Position - dragStart
                local vp = workspace.CurrentCamera.ViewportSize
                win.Position = UDim2.new(0, math.clamp(startPos.X.Offset+d.X,0,vp.X-WIN_W), 0, math.clamp(startPos.Y.Offset+d.Y,0,vp.Y-WIN_H))
            end
        end)
        UserInputService.InputEnded:Connect(function(i)
            if i.UserInputType == Enum.UserInputType.MouseButton1 or i.UserInputType == Enum.UserInputType.Touch then
                dragging = false
            end
        end)
    end
    
    local function makePage(parent, color)
        local pg = Instance.new("ScrollingFrame")
        pg.Size = UDim2.new(1,0,1,0)
        pg.BackgroundTransparency = 1
        pg.BorderSizePixel = 0
        pg.ScrollBarThickness = 4
        pg.ScrollBarImageColor3 = color
        pg.ScrollBarImageTransparency = 0.5
        pg.CanvasSize = UDim2.new(0,0,0,0)
        pg.AutomaticCanvasSize = Enum.AutomaticSize.Y
        pg.ScrollingDirection = Enum.ScrollingDirection.Y
        pg.Visible = false
        pg.ZIndex = 2
        pg.Parent = parent
        
        local layout = Instance.new("UIListLayout")
        layout.SortOrder = Enum.SortOrder.LayoutOrder
        layout.HorizontalAlignment = Enum.HorizontalAlignment.Center
        layout.Padding = UDim.new(0,8)
        layout.Parent = pg
        return pg
    end
    
    -- ==========================================
    -- NOTIFICATION
    -- ==========================================
    local function notify(title, msg, nType)
        local col = nType=="success" and green or nType=="error" and red or nType=="warning" and gold or accent
        local n = Instance.new("Frame")
        n.Size = UDim2.new(0,0,0,58)
        n.Position = UDim2.new(1,-10,0.95,-68)
        n.AnchorPoint = Vector2.new(1,1)
        n.BackgroundColor3 = T.bg
        n.BorderSizePixel = 0
        n.Parent = sg
        Instance.new("UICorner", n).CornerRadius = UDim.new(0,8)
        
        local bar = Instance.new("Frame")
        bar.Size = UDim2.new(0,4,1,0)
        bar.BackgroundColor3 = col
        bar.BorderSizePixel = 0
        bar.Parent = n
        Instance.new("UICorner", bar).CornerRadius = UDim.new(0,8)
        
        local t1 = Instance.new("TextLabel")
        t1.Size = UDim2.new(1,-16,0,18)
        t1.Position = UDim2.new(0,12,0,7)
        t1.Text = title
        t1.Font = Enum.Font.GothamBold
        t1.TextSize = 11
        t1.TextColor3 = T.text
        t1.TextXAlignment = Enum.TextXAlignment.Left
        t1.BackgroundTransparency = 1
        t1.Parent = n
        
        local t2 = Instance.new("TextLabel")
        t2.Size = UDim2.new(1,-16,0,22)
        t2.Position = UDim2.new(0,12,0,26)
        t2.Text = msg
        t2.Font = Enum.Font.Gotham
        t2.TextSize = 9
        t2.TextColor3 = T.text
        t2.TextTransparency = 0.35
        t2.TextXAlignment = Enum.TextXAlignment.Left
        t2.TextWrapped = true
        t2.BackgroundTransparency = 1
        t2.Parent = n
        
        TweenService:Create(n,tw(),{Size=UDim2.new(0,280,0,58)}):Play()
        
        task.delay(3, function()
            TweenService:Create(n,tw(),{Size=UDim2.new(0,0,0,58),BackgroundTransparency=1}):Play()
            task.wait(0.3)
            if n and n.Parent then n:Destroy() end
        end)
    end
    
    -- ==========================================
    -- BUILD IRC WINDOW
    -- ==========================================
    local ircWin, ircStroke = makeWin(IRC_POS)
    
    local IRC_TABS = {
        {name="CHAT", icon="💬", color=accent},
        {name="MEMBERS", icon="👥", color=blue},
    }
    
    local ircPages = {}
    local ircNavBtns = {}
    local ircActiveTab = 1
    
    local function setIRCTab(idx)
        ircActiveTab = idx
        for i,btn in ipairs(ircNavBtns) do
            local on = (i==idx)
            TweenService:Create(btn.bg,tw(0.15),{BackgroundColor3=on and btn.color or T.dim, BackgroundTransparency=on and 0.15 or 0}):Play()
            TweenService:Create(btn.lbl,tw(0.15),{TextColor3=on and btn.color or T.muted}):Play()
            TweenService:Create(btn.ico,tw(0.15),{TextTransparency=on and 0 or 0.35}):Play()
            btn.bar.BackgroundTransparency = on and 0 or 1
            ircPages[i].Visible = on
        end
    end
    
    local ircSB, ircNB = makeIRCSidebar(ircWin, IRC_TABS, setIRCTab)
    ircNavBtns = ircNB
    
    local ircHdr, ircTitle, ircClose, ircMinBtn = makeHeader(ircWin, "Feather IRC")
    makeDrag(ircWin, ircHdr)
    
    local ircContent = makeContent(ircWin)
    for i,def in ipairs(IRC_TABS) do
        local pg = makePage(ircContent, def.color)
        pg.Visible = (i==1)
        ircPages[i] = pg
    end
    
    -- ==========================================
    -- CHAT PAGE
    -- ==========================================
    local chatPg = ircPages[1]
    
    -- Status row
    local ircStatRow = Instance.new("Frame")
    ircStatRow.Size = UDim2.new(1,0,0,24)
    ircStatRow.BackgroundColor3 = T.dim
    ircStatRow.BorderSizePixel = 0
    ircStatRow.LayoutOrder = 1
    ircStatRow.ZIndex = 4
    ircStatRow.Parent = chatPg
    Instance.new("UICorner", ircStatRow).CornerRadius = UDim.new(0,7)
    reg(ircStatRow,"BackgroundColor3",LIGHT.dim,DARK.dim)
    
    local onlineDot = Instance.new("Frame")
    onlineDot.Size = UDim2.new(0,7,0,7)
    onlineDot.Position = UDim2.new(0,9,0.5,-3.5)
    onlineDot.BackgroundColor3 = T.muted
    onlineDot.BorderSizePixel = 0
    onlineDot.ZIndex = 5
    onlineDot.Parent = ircStatRow
    Instance.new("UICorner", onlineDot).CornerRadius = UDim.new(1,0)
    
    local ircStatLbl = Instance.new("TextLabel")
    ircStatLbl.Size = UDim2.new(1,-24,1,0)
    ircStatLbl.Position = UDim2.new(0,22,0,0)
    ircStatLbl.Text = "Connecting..."
    ircStatLbl.Font = Enum.Font.GothamMedium
    ircStatLbl.TextSize = isMobile and 9 or 8
    ircStatLbl.TextColor3 = T.muted
    ircStatLbl.BackgroundTransparency = 1
    ircStatLbl.TextXAlignment = Enum.TextXAlignment.Left
    ircStatLbl.ZIndex = 5
    ircStatLbl.Parent = ircStatRow
    reg(ircStatLbl,"TextColor3",LIGHT.muted,DARK.muted)
    
    -- Message scroll
    local CONTENT_H = WIN_H - 42
    local INPUT_H = isMobile and 40 or 34
    local STAT_H = 24
    local MSG_H = CONTENT_H - STAT_H - INPUT_H - 22
    
    local msgScroll = Instance.new("ScrollingFrame")
    msgScroll.Size = UDim2.new(1,0,0,MSG_H)
    msgScroll.LayoutOrder = 2
    msgScroll.BackgroundColor3 = T.card
    msgScroll.BorderSizePixel = 0
    msgScroll.ScrollBarThickness = 3
    msgScroll.ScrollBarImageColor3 = accent
    msgScroll.CanvasSize = UDim2.new(0,0,0,0)
    msgScroll.AutomaticCanvasSize = Enum.AutomaticSize.Y
    msgScroll.ScrollingDirection = Enum.ScrollingDirection.Y
    msgScroll.ElasticBehavior = Enum.ElasticBehavior.Never
    msgScroll.ZIndex = 4
    msgScroll.Parent = chatPg
    Instance.new("UICorner", msgScroll).CornerRadius = UDim.new(0,8)
    
    local msS = Instance.new("UIStroke")
    msS.Color = T.border
    msS.Thickness = 1
    msS.Parent = msgScroll
    reg(msgScroll,"BackgroundColor3",LIGHT.card,DARK.card)
    reg(msS,"Color",LIGHT.border,DARK.border)
    
    local mList = Instance.new("UIListLayout")
    mList.SortOrder = Enum.SortOrder.LayoutOrder
    mList.Padding = UDim.new(0,2)
    mList.Parent = msgScroll
    
    local mPad = Instance.new("UIPadding")
    mPad.PaddingTop = UDim.new(0,4)
    mPad.PaddingBottom = UDim.new(0,4)
    mPad.PaddingLeft = UDim.new(0,4)
    mPad.PaddingRight = UDim.new(0,4)
    mPad.Parent = msgScroll
    
    -- Input
    local inputRow = Instance.new("Frame")
    inputRow.Size = UDim2.new(1,0,0,INPUT_H)
    inputRow.LayoutOrder = 3
    inputRow.BackgroundColor3 = T.inputBg
    inputRow.BorderSizePixel = 0
    inputRow.ZIndex = 4
    inputRow.Parent = chatPg
    Instance.new("UICorner", inputRow).CornerRadius = UDim.new(0,8)
    
    local inpS = Instance.new("UIStroke")
    inpS.Color = T.border
    inpS.Thickness = 1
    inpS.Parent = inputRow
    reg(inputRow,"BackgroundColor3",LIGHT.inputBg,DARK.inputBg)
    reg(inpS,"Color",LIGHT.border,DARK.border)
    
    local SEND_W = isMobile and 38 or 32
    local SEND_H = isMobile and 28 or 24
    local TB_H = INPUT_H - (isMobile and 12 or 10)
    
    local textBox = Instance.new("TextBox")
    textBox.Size = UDim2.new(1,-(SEND_W+16),0,TB_H)
    textBox.Position = UDim2.new(0,8,0.5,-math.floor(TB_H/2))
    textBox.PlaceholderText = "Message everyone..."
    textBox.Text = ""
    textBox.Font = Enum.Font.GothamMedium
    textBox.TextSize = isMobile and 11 or 9
    textBox.TextColor3 = T.text
    textBox.PlaceholderColor3 = T.muted
    textBox.BackgroundTransparency = 1
    textBox.BorderSizePixel = 0
    textBox.ClearTextOnFocus = false
    textBox.ZIndex = 5
    textBox.Parent = inputRow
    reg(textBox,"TextColor3",LIGHT.text,DARK.text)
    reg(textBox,"PlaceholderColor3",LIGHT.muted,DARK.muted)
    
    local sendBtn = Instance.new("TextButton")
    sendBtn.Size = UDim2.new(0,SEND_W,0,SEND_H)
    sendBtn.Position = UDim2.new(1,-(SEND_W+4),0.5,-math.floor(SEND_H/2))
    sendBtn.Text = "➤"
    sendBtn.Font = Enum.Font.GothamBold
    sendBtn.TextSize = isMobile and 13 or 11
    sendBtn.TextColor3 = Color3.new(1,1,1)
    sendBtn.BackgroundColor3 = accent
    sendBtn.BorderSizePixel = 0
    sendBtn.AutoButtonColor = false
    sendBtn.ZIndex = 5
    sendBtn.Parent = inputRow
    Instance.new("UICorner", sendBtn).CornerRadius = UDim.new(0,6)
    
    -- ==========================================
    -- MEMBERS PAGE
    -- ==========================================
    local membersPg = ircPages[2]
    
    local memberList = Instance.new("Frame")
    memberList.Size = UDim2.new(1,0,0,0)
    memberList.AutomaticSize = Enum.AutomaticSize.Y
    memberList.BackgroundColor3 = T.card
    memberList.BorderSizePixel = 0
    memberList.LayoutOrder = 1
    memberList.ZIndex = 3
    memberList.Parent = membersPg
    Instance.new("UICorner", memberList).CornerRadius = UDim.new(0,10)
    
    local mlS = Instance.new("UIStroke")
    mlS.Color = T.border
    mlS.Thickness = 1
    mlS.Parent = memberList
    reg(memberList,"BackgroundColor3",LIGHT.card,DARK.card)
    reg(mlS,"Color",LIGHT.border,DARK.border)
    
    local mlHeader = Instance.new("TextLabel")
    mlHeader.Size = UDim2.new(1,0,0,28)
    mlHeader.LayoutOrder = 0
    mlHeader.Text = "Recent Participants"
    mlHeader.Font = Enum.Font.GothamBold
    mlHeader.TextSize = 9
    mlHeader.TextColor3 = T.muted
    mlHeader.BackgroundColor3 = T.dim
    mlHeader.BackgroundTransparency = 0
    mlHeader.BorderSizePixel = 0
    mlHeader.ZIndex = 4
    mlHeader.Parent = memberList
    Instance.new("UICorner", mlHeader).CornerRadius = UDim.new(0,8)
    reg(mlHeader,"BackgroundColor3",LIGHT.dim,DARK.dim)
    reg(mlHeader,"TextColor3",LIGHT.muted,DARK.muted)
    
    local memberEntries = {}
    
    -- ==========================================
    -- SIDEBAR BOTTOM BUTTONS
    -- ==========================================
    local bottomContainer = Instance.new("Frame")
    bottomContainer.Size = UDim2.new(1,-12,0,90)
    bottomContainer.Position = UDim2.new(0,6,1,-95)
    bottomContainer.BackgroundTransparency = 1
    bottomContainer.ZIndex = 5
    bottomContainer.Parent = ircSB
    
    local bcLayout = Instance.new("UIListLayout")
    bcLayout.SortOrder = Enum.SortOrder.LayoutOrder
    bcLayout.VerticalAlignment = Enum.VerticalAlignment.Bottom
    bcLayout.HorizontalAlignment = Enum.HorizontalAlignment.Center
    bcLayout.Padding = UDim.new(0,6)
    bcLayout.Parent = bottomContainer
    
    -- Feather Hub text
    local wmN = Instance.new("TextLabel")
    wmN.Size = UDim2.new(1,0,0,14)
    wmN.Text = "feather hub"
    wmN.Font = Enum.Font.GothamBold
    wmN.TextSize = 8
    wmN.TextColor3 = accent
    wmN.BackgroundTransparency = 1
    wmN.TextXAlignment = Enum.TextXAlignment.Center
    wmN.ZIndex = 5
    wmN.Parent = bottomContainer
    
    -- Discord Button
    local discordBtn = Instance.new("TextButton")
    discordBtn.Size = UDim2.new(1,0,0,28)
    discordBtn.BackgroundColor3 = discordColor
    discordBtn.BorderSizePixel = 0
    discordBtn.Text = ""
    discordBtn.AutoButtonColor = false
    discordBtn.ZIndex = 5
    discordBtn.Parent = bottomContainer
    Instance.new("UICorner", discordBtn).CornerRadius = UDim.new(0,6)
    
    local discordIcon = Instance.new("TextLabel")
    discordIcon.Size = UDim2.new(0,20,1,0)
    discordIcon.Position = UDim2.new(0,6,0,0)
    discordIcon.Text = "🔗"
    discordIcon.Font = Enum.Font.GothamBold
    discordIcon.TextSize = 12
    discordIcon.TextColor3 = Color3.new(1,1,1)
    discordIcon.BackgroundTransparency = 1
    discordIcon.ZIndex = 6
    discordIcon.Parent = discordBtn
    
    local discordText = Instance.new("TextLabel")
    discordText.Size = UDim2.new(1,-26,1,0)
    discordText.Position = UDim2.new(0,26,0,0)
    discordText.Text = "Join Discord"
    discordText.Font = Enum.Font.GothamBold
    discordText.TextSize = 8
    discordText.TextColor3 = Color3.new(1,1,1)
    discordText.BackgroundTransparency = 1
    discordText.TextXAlignment = Enum.TextXAlignment.Left
    discordText.ZIndex = 6
    discordText.Parent = discordBtn
    
    discordBtn.MouseButton1Click:Connect(function()
        pcall(function() if setclipboard then setclipboard(DISCORD_INVITE) end end)
        notify("Discord Copied!", "Join our community for updates", "success")
    end)
    
    -- FPS Unlock Button
    local fpsBtn = Instance.new("TextButton")
    fpsBtn.Size = UDim2.new(1,0,0,28)
    fpsBtn.BackgroundColor3 = T.dim
    fpsBtn.BorderSizePixel = 0
    fpsBtn.Text = ""
    fpsBtn.AutoButtonColor = false
    fpsBtn.ZIndex = 5
    fpsBtn.Parent = bottomContainer
    Instance.new("UICorner", fpsBtn).CornerRadius = UDim.new(0,6)
    
    local fpsS = Instance.new("UIStroke")
    fpsS.Color = orange
    fpsS.Thickness = 1
    fpsS.Parent = fpsBtn
    
    local fpsIcon = Instance.new("TextLabel")
    fpsIcon.Size = UDim2.new(0,20,1,0)
    fpsIcon.Position = UDim2.new(0,6,0,0)
    fpsIcon.Text = "⚡"
    fpsIcon.Font = Enum.Font.GothamBold
    fpsIcon.TextSize = 12
    fpsIcon.TextColor3 = orange
    fpsIcon.BackgroundTransparency = 1
    fpsIcon.ZIndex = 6
    fpsIcon.Parent = fpsBtn
    
    local fpsText = Instance.new("TextLabel")
    fpsText.Size = UDim2.new(1,-40,1,0)
    fpsText.Position = UDim2.new(0,26,0,0)
    fpsText.Text = "Unlock FPS"
    fpsText.Font = Enum.Font.GothamBold
    fpsText.TextSize = 8
    fpsText.TextColor3 = orange
    fpsText.BackgroundTransparency = 1
    fpsText.TextXAlignment = Enum.TextXAlignment.Left
    fpsText.ZIndex = 6
    fpsText.Parent = fpsBtn
    
    local fpsStatus = Instance.new("TextLabel")
    fpsStatus.Size = UDim2.new(0,30,1,0)
    fpsStatus.Position = UDim2.new(1,-32,0,0)
    fpsStatus.Text = "OFF"
    fpsStatus.Font = Enum.Font.GothamBold
    fpsStatus.TextSize = 7
    fpsStatus.TextColor3 = T.muted
    fpsStatus.BackgroundTransparency = 1
    fpsStatus.TextXAlignment = Enum.TextXAlignment.Right
    fpsStatus.ZIndex = 6
    fpsStatus.Parent = fpsBtn
    
    fpsBtn.MouseButton1Click:Connect(function()
        toggleFPSUnlock()
        fpsStatus.Text = fpsUnlocked and "ON" or "OFF"
        fpsStatus.TextColor3 = fpsUnlocked and green or T.muted
        fpsText.TextColor3 = fpsUnlocked and green or orange
        fpsIcon.TextColor3 = fpsUnlocked and green or orange
        fpsS.Color = fpsUnlocked and green or orange
    end)
    
    -- Theme Toggle Button
    local thBtn = Instance.new("TextButton")
    thBtn.Size = UDim2.new(1,0,0,24)
    thBtn.BackgroundColor3 = T.dim
    thBtn.BorderSizePixel = 0
    thBtn.Text = "Dark"
    thBtn.Font = Enum.Font.GothamBold
    thBtn.TextSize = 8
    thBtn.TextColor3 = T.muted
    thBtn.AutoButtonColor = false
    thBtn.ZIndex = 5
    thBtn.Parent = bottomContainer
    Instance.new("UICorner", thBtn).CornerRadius = UDim.new(0,6)
    
    local thS = Instance.new("UIStroke")
    thS.Color = T.border
    thS.Thickness = 1
    thS.Parent = thBtn
    reg(thBtn,"BackgroundColor3",LIGHT.dim,DARK.dim)
    reg(thBtn,"TextColor3",LIGHT.muted,DARK.muted)
    reg(thS,"Color",LIGHT.border,DARK.border)
    
    local function doTheme()
        isDark = not isDark
        T = isDark and DARK or LIGHT
        applyThemeReg()
        thBtn.Text = isDark and "Light" or "Dark"
    end
    
    thBtn.MouseButton1Click:Connect(doTheme)
    
    -- ==========================================
    -- MINIMIZE FUNCTIONALITY
    -- ==========================================
    local minimized = false
    local originalSize = UDim2.new(0,WIN_W,0,WIN_H)
    local minimizedSize = UDim2.new(0,WIN_W,0,42)
    
    ircMinBtn.MouseButton1Click:Connect(function()
        minimized = not minimized
        if minimized then
            TweenService:Create(ircWin,tw(0.3,Enum.EasingStyle.Quad,Enum.EasingDirection.Out),{Size=minimizedSize}):Play()
            ircMinBtn.Text = "□"
            ircContent.Visible = false
            ircSB.Visible = false
        else
            TweenService:Create(ircWin,tw(0.3,Enum.EasingStyle.Quad,Enum.EasingDirection.Out),{Size=originalSize}):Play()
            ircMinBtn.Text = "−"
            ircContent.Visible = true
            ircSB.Visible = true
            ircPages[ircActiveTab].Visible = true
        end
    end)
    
    ircClose.MouseButton1Click:Connect(function()
        TweenService:Create(ircWin,tw(0.2,Enum.EasingStyle.Back,Enum.EasingDirection.In),{Size=UDim2.new(0,0,0,0)}):Play()
        task.wait(0.2)
        ircWin:Destroy()
    end)
    
    -- ==========================================
    -- IRC MESSAGES
    -- ==========================================
    local renderedIds = {}
    local msgOrder = 0
    
    local function scrollToBottom()
        task.defer(function()
            local maxY = msgScroll.AbsoluteCanvasSize.Y - msgScroll.AbsoluteSize.Y
            if maxY > 0 then
                msgScroll.CanvasPosition = Vector2.new(0,maxY+9999)
            end
        end)
    end
    
    local function addMessageBubble(d)
        if renderedIds[d.id] then return end
        renderedIds[d.id] = true
        msgOrder = msgOrder + 1
        
        local isSelf = (d.username == Username)
        local isSys = (d.system == true)
        local nameCol = isSys and gold or getUserColor(d.username or "?")
        
        local row = Instance.new("Frame")
        row.Size = UDim2.new(1,0,0,0)
        row.AutomaticSize = Enum.AutomaticSize.Y
        row.BackgroundTransparency = 1
        row.LayoutOrder = msgOrder
        row.BorderSizePixel = 0
        row.ZIndex = 4
        row.Parent = msgScroll
        
        local bubble = Instance.new("Frame")
        bubble.Size = UDim2.new(0.92,0,0,0)
        bubble.AutomaticSize = Enum.AutomaticSize.Y
        bubble.Position = isSelf and UDim2.new(0.08,0,0,0) or UDim2.new(0,0,0,0)
        bubble.BackgroundColor3 = isSelf and T.msgSelf or (isSys and (isDark and Color3.fromRGB(50,45,20) or Color3.fromRGB(255,252,230)) or T.msgOther)
        bubble.BorderSizePixel = 0
        bubble.ZIndex = 5
        bubble.Parent = row
        Instance.new("UICorner", bubble).CornerRadius = UDim.new(0,6)
        
        reg(bubble,"BackgroundColor3",
            isSelf and LIGHT.msgSelf or (isSys and Color3.fromRGB(255,252,230) or LIGHT.msgOther),
            isSelf and DARK.msgSelf or (isSys and Color3.fromRGB(50,45,20) or DARK.msgOther))
        
        local strip = Instance.new("Frame")
        strip.Size = UDim2.new(0,3,1,0)
        strip.BackgroundColor3 = nameCol
        strip.BorderSizePixel = 0
        strip.ZIndex = 6
        strip.Parent = bubble
        Instance.new("UICorner", strip).CornerRadius = UDim.new(0,3)
        
        local bPad = Instance.new("UIPadding")
        bPad.PaddingLeft = UDim.new(0,10)
        bPad.PaddingRight = UDim.new(0,6)
        bPad.PaddingTop = UDim.new(0,4)
        bPad.PaddingBottom = UDim.new(0,4)
        bPad.Parent = bubble
        
        local bL = Instance.new("UIListLayout")
        bL.SortOrder = Enum.SortOrder.LayoutOrder
        bL.Padding = UDim.new(0,1)
        bL.Parent = bubble
        
        local hRow = Instance.new("Frame")
        hRow.Size = UDim2.new(1,0,0,13)
        hRow.BackgroundTransparency = 1
        hRow.LayoutOrder = 1
        hRow.ZIndex = 6
        hRow.Parent = bubble
        
        local nameStr = isSys and "✦ SYSTEM" or (isSelf and "you" or tostring(d.user or d.username))
        local nLbl = Instance.new("TextLabel")
        nLbl.Size = UDim2.new(0.75,0,1,0)
        nLbl.Text = nameStr
        nLbl.Font = Enum.Font.GothamBold
        nLbl.TextSize = isMobile and 9 or 8
        nLbl.TextColor3 = nameCol
        nLbl.TextXAlignment = Enum.TextXAlignment.Left
        nLbl.TextTruncate = Enum.TextTruncate.AtEnd
        nLbl.BackgroundTransparency = 1
        nLbl.ZIndex = 6
        nLbl.Parent = hRow
        
        local tLbl = Instance.new("TextLabel")
        tLbl.Size = UDim2.new(0.25,0,1,0)
        tLbl.Position = UDim2.new(0.75,0,0,0)
        tLbl.Text = tostring(d.time or "")
        tLbl.Font = Enum.Font.Gotham
        tLbl.TextSize = isMobile and 7 or 6
        tLbl.TextColor3 = T.muted
        tLbl.TextXAlignment = Enum.TextXAlignment.Right
        tLbl.BackgroundTransparency = 1
        tLbl.ZIndex = 6
        tLbl.Parent = hRow
        reg(tLbl,"TextColor3",LIGHT.muted,DARK.muted)
        
        local bodyLbl = Instance.new("TextLabel")
        bodyLbl.Size = UDim2.new(1,0,0,0)
        bodyLbl.AutomaticSize = Enum.AutomaticSize.Y
        bodyLbl.Text = tostring(d.text or "")
        bodyLbl.Font = Enum.Font.GothamMedium
        bodyLbl.TextSize = isMobile and 10 or 9
        bodyLbl.TextColor3 = isSys and gold or T.text
        bodyLbl.TextXAlignment = Enum.TextXAlignment.Left
        bodyLbl.TextWrapped = true
        bodyLbl.BackgroundTransparency = 1
        bodyLbl.LayoutOrder = 2
        bodyLbl.ZIndex = 6
        bodyLbl.Parent = bubble
        if not isSys then reg(bodyLbl,"TextColor3",LIGHT.text,DARK.text) end
        
        scrollToBottom()
    end
    
    local function addSysMsg(text)
        local t = os.date("*t")
        addMessageBubble({
            id = string.format("%.0f_sys",tick()*1000),
            user = "System",
            username = "_system_",
            text = tostring(text),
            time = string.format("%02d:%02d",t.hour,t.min),
            system = true
        })
    end
    
    -- ==========================================
    -- IRC HTTP
    -- ==========================================
    local function httpReq(method,url,headers,body)
        body = body or ""
        local opts = {Url=url, Method=method, Headers=headers, Body=body}
        local function try(fn)
            if not fn then return nil end
            local ok,res = pcall(fn,opts)
            if ok and type(res)=="table" and res.Body and res.Body~="" then return res end
            return nil
        end
        local res = nil
        pcall(function() if syn and syn.request then res = try(syn.request) end end)
        if not res then pcall(function() if http and http.request then res = try(http.request) end end) end
        if not res then pcall(function() if request then res = try(request) end end) end
        if not res then pcall(function() if fluxus and fluxus.request then res = try(fluxus.request) end end) end
        return res
    end
    
    local function readMsgs()
        local res = httpReq("GET",IRC_BASE_URL..IRC_BIN_ID.."/latest",{["X-Access-Key"]=IRC_ACCESS_KEY, ["X-Bin-Meta"]="false"})
        if not res then return nil end
        local ok,data = pcall(HttpService.JSONDecode,HttpService,res.Body)
        if not ok or not data then return nil end
        local record = data.record or data
        if type(record)=="table" and type(record.messages)=="table" then return record.messages end
        return nil
    end
    
    local function writeMsgs(msgs)
        local body = HttpService:JSONEncode({messages=msgs})
        local res = httpReq("PUT",IRC_BASE_URL..IRC_BIN_ID,{["Content-Type"]="application/json", ["X-Access-Key"]=IRC_ACCESS_KEY},body)
        if not res then return false end
        local ok,data = pcall(HttpService.JSONDecode,HttpService,res.Body)
        return ok and data and not data["error"]
    end
    
    local isSending = false
    
    local function sendMessage(text)
        text = tostring(text):match("^%s*(.-)%s*$")
        if text=="" or #text>200 then return end
        if isSending then return end
        isSending = true
        
        local t = os.date("*t")
        local msgId = string.format("%.0f",tick()*1000).."_"..Username
        local timeStr = string.format("%02d:%02d",t.hour,t.min)
        
        addMessageBubble({id=msgId, user=DisplayName, username=Username, text=text, time=timeStr})
        textBox.Text = ""
        
        task.spawn(function()
            local msgs = readMsgs() or {}
            table.insert(msgs,{id=msgId, user=DisplayName, username=Username, text=text, time=timeStr})
            while #msgs>IRC_MAX_MSGS do table.remove(msgs,1) end
            if not writeMsgs(msgs) then addSysMsg("⚠ Message may not have saved") end
            isSending = false
        end)
    end
    
    local function onSend()
        local txt = textBox.Text
        if txt and txt~="" then sendMessage(txt) end
    end
    
    sendBtn.MouseButton1Click:Connect(onSend)
    textBox.FocusLost:Connect(function(e) if e then onSend() end end)
    
    local function updateMembers(msgs)
        local seen = {}
        local order = {}
        for _,m in ipairs(msgs) do
            if m.username and m.username~="_system_" and not seen[m.username] then
                seen[m.username]=m
                table.insert(order,m.username)
            end
        end
        for _,e in pairs(memberEntries) do if e and e.Parent then e:Destroy() end end
        memberEntries = {}
        for i,uname in ipairs(order) do
            local m = seen[uname]
            local row = Instance.new("Frame")
            row.Size = UDim2.new(1,0,0,36)
            row.BackgroundTransparency = 1
            row.BorderSizePixel = 0
            row.LayoutOrder = i
            row.ZIndex = 4
            row.Parent = memberList
            
            local dot = Instance.new("Frame")
            dot.Size = UDim2.new(0,8,0,8)
            dot.Position = UDim2.new(0,12,0.5,-4)
            dot.BackgroundColor3 = getUserColor(uname)
            dot.BorderSizePixel = 0
            dot.ZIndex = 5
            dot.Parent = row
            Instance.new("UICorner", dot).CornerRadius = UDim.new(1,0)
            
            local nL = Instance.new("TextLabel")
            nL.Size = UDim2.new(1,-60,0,16)
            nL.Position = UDim2.new(0,26,0,4)
            nL.Text = tostring(m.user or uname)
            nL.Font = Enum.Font.GothamSemibold
            nL.TextSize = 10
            nL.TextColor3 = T.text
            nL.BackgroundTransparency = 1
            nL.TextXAlignment = Enum.TextXAlignment.Left
            nL.ZIndex = 5
            nL.Parent = row
            reg(nL,"TextColor3",LIGHT.text,DARK.text)
            
            local uL = Instance.new("TextLabel")
            uL.Size = UDim2.new(1,-60,0,12)
            uL.Position = UDim2.new(0,26,0,20)
            uL.Text = "@"..uname
            uL.Font = Enum.Font.Gotham
            uL.TextSize = 8
            uL.TextColor3 = getUserColor(uname)
            uL.BackgroundTransparency = 1
            uL.TextXAlignment = Enum.TextXAlignment.Left
            uL.ZIndex = 5
            uL.Parent = row
            
            table.insert(memberEntries,row)
        end
    end
    
    local function syncIRC()
        local msgs = readMsgs()
        if not msgs then
            ircStatLbl.Text = "⚠ Retrying..."
            ircStatLbl.TextColor3 = red
            onlineDot.BackgroundColor3 = red
            return
        end
        onlineDot.BackgroundColor3 = green
        
        local seen = {}
        for _,m in ipairs(msgs) do if m.username and m.username~="_system_" then seen[m.username]=true end end
        local n = 0
        for _ in pairs(seen) do n = n+1 end
        ircStatLbl.Text = n.." user"..(n==1 and "" or "s").."  •  live"
        ircStatLbl.TextColor3 = green
        
        updateMembers(msgs)
        
        for _,msg in ipairs(msgs) do
            if msg.id and msg.text and msg.username then
                addMessageBubble({
                    id=tostring(msg.id),
                    user=tostring(msg.user or msg.username),
                    username=tostring(msg.username),
                    text=tostring(msg.text),
                    time=tostring(msg.time or ""),
                    system=(msg.system==true)
                })
            end
        end
    end
    
    -- ==========================================
    -- INITIALIZE
    -- ==========================================
    setIRCTab(1)
    
    task.spawn(function()
        task.wait(0.6)
        addSysMsg("Welcome, "..DisplayName.." (@"..Username..")")
        syncIRC()
        
        task.spawn(function()
            task.wait(1)
            local msgs = readMsgs() or {}
            local t = os.date("*t")
            table.insert(msgs,{
                id=string.format("%.0f",tick()*1000).."_join_"..Username,
                user="System",
                username="_system_",
                text=DisplayName.." (@"..Username..") joined the hub",
                time=string.format("%02d:%02d",t.hour,t.min),
                system=true
            })
            while #msgs>IRC_MAX_MSGS do table.remove(msgs,1) end
            writeMsgs(msgs)
        end)
        
        while task.wait(IRC_POLL_RATE) do
            if not sg or not sg.Parent then break end
            syncIRC()
        end
    end)
    
    -- Open animation
    ircWin.Size = UDim2.new(0,0,0,0)
    ircWin.BackgroundTransparency = 1
    TweenService:Create(ircWin, tw(0.38,Enum.EasingStyle.Back,Enum.EasingDirection.Out), {Size=UDim2.new(0,WIN_W,0,WIN_H), BackgroundTransparency=0}):Play()
    
    print("[Feather Hub] IRC loaded")
end
