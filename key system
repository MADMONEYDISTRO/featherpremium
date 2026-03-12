--[[
    ════════════════════════════════════════════════════
     FEATHER HUB  —  Key System & Script Selector
     Load this separately from IRC
    ════════════════════════════════════════════════════
]]

return function(sg)
    -- Services
    local TweenService = game:GetService("TweenService")
    local UserInputService = game:GetService("UserInputService")
    local Players = game:GetService("Players")
    local MarketplaceService = game:GetService("MarketplaceService")
    local RunService = game:GetService("RunService")
    local Stats = game:GetService("Stats")
    
    local Player = Players.LocalPlayer
    local DisplayName = tostring(Player.DisplayName)
    local Username = tostring(Player.Name)
    local UserId = Player.UserId
    
    -- Device detection
    local isMobile = UserInputService.TouchEnabled and not UserInputService.KeyboardEnabled
    
    -- ==========================================
    -- CONFIGURATION
    -- ==========================================
    local DISCORD_INVITE = "https://discord.gg/7P8Jaa9fp8"
    local WEBHOOK_URL = "https://discord.com/api/webhooks/1481416163287105626/C1a1vMaWHxKrVX1C3knVebeHqsUCj19NcWZ8W1I7FPYt9YHiYn8pXVcUhTFHAYFmQXOP"
    local PREMIUM_WEBHOOK = "https://discord.com/api/webhooks/1481416163287105626/C1a1vMaWHxKrVX1C3knVebeHqsUCj19NcWZ8W1I7FPYt9YHiYn8pXVcUhTFHAYFmQXOP"
    
    local VALID_KEYS = {
        ["mad"] = true,
        ["test"] = true,
        ["premium2024"] = true,
        ["vip_access"] = true,
        ["feather_pro"] = true,
    }
    
    local FREE_SCRIPTS = {
        { name="Universal",   desc="Works on most games", icon="⚔️", tag="FREE", url="https://raw.githubusercontent.com/MADMONEYDISTRO/madmoney-ghost-client/refs/heads/main/good%20kill%20all%20fr" },
        { name="Drive World", desc="Autofarm + trailer delivery", icon="🚗", tag="FREE", url="https://raw.githubusercontent.com/MADMONEYDISTRO/feather-hub/refs/heads/main/test" },
        { name="Notoriety", desc="Only working script for this game", icon="💰", tag="FREE", url="https://raw.githubusercontent.com/MADMONEYDISTRO/madmoney-ghost-client/refs/heads/main/dsasd" },
        { name="BGSI Event Farm", desc="Auto flower farm", icon="🌸", tag="FREE", url="https://raw.githubusercontent.com/MADMONEYDISTRO/feather-hub/refs/heads/main/flower" },
        { name="99 Nights", desc="Kill aura, ESP/TP", icon="🏕️", tag="FREE", url="https://raw.githubusercontent.com/MADMONEYDISTRO/feather-hub/refs/heads/main/99" },
        { name="Tha Bronx Item Dupe", desc="Hold item then press dupe", icon="🔄", tag="FREE", url="https://raw.githubusercontent.com/MADMONEYDISTRO/feather-hub/refs/heads/main/dupe" },
        { name="Blade Ball", desc="Auto parry, auto dodge", icon="⚽", tag="FREE", url="https://raw.githubusercontent.com/MADMONEYDISTRO/feather-hub/refs/heads/main/bladeball" },
        { name="Pet Sim 99", desc="Auto farm, auto hatch", icon="🐾", tag="FREE", url="https://raw.githubusercontent.com/MADMONEYDISTRO/feather-hub/refs/heads/main/petsim" },
        { name="Tower Defense", desc="Auto place, instant win", icon="🏰", tag="FREE", url="https://raw.githubusercontent.com/MADMONEYDISTRO/feather-hub/refs/heads/main/tds" },
        { name="Fisch", desc="Auto fish, instant catch", icon="🎣", tag="FREE", url="https://raw.githubusercontent.com/MADMONEYDISTRO/feather-hub/refs/heads/main/fisch" },
    }
    
    local PREMIUM_SCRIPTS = {
        { name="Legends of Speed", desc="OP Farm (1B+ per hour)", icon="⚡", tag="PRIVATE", url="https://raw.githubusercontent.com/MADMONEYDISTRO/feather-hub/refs/heads/main/test" },
        { name="Fishing Sim", desc="Chest TP + Auto Fish", icon="🎣", tag="PRIVATE", url="https://raw.githubusercontent.com/MADMONEYDISTRO/v2/refs/heads/main/fishing%20sim" },
        { name="Blox Fruits", desc="Auto Farm + Fruit Sniper", icon="🍎", tag="PRIVATE", url="https://raw.githubusercontent.com/MADMONEYDISTRO/feather-hub/refs/heads/main/bloxfruits" },
        { name="BedWars", desc="Anti Void + Kill Aura", icon="🛏️", tag="PRIVATE", url="https://raw.githubusercontent.com/MADMONEYDISTRO/feather-hub/refs/heads/main/bedwars" },
    }
    
    -- ==========================================
    -- COLOR PALETTE
    -- ==========================================
    local DARK = {
        bg = Color3.fromRGB(14,14,16), sidebar = Color3.fromRGB(20,20,24),
        card = Color3.fromRGB(26,26,30), border = Color3.fromRGB(44,44,52),
        text = Color3.fromRGB(240,240,244), muted = Color3.fromRGB(100,100,115),
        dim = Color3.fromRGB(32,32,38), sep = Color3.fromRGB(36,36,44),
        header = Color3.fromRGB(18,18,22), inputBg = Color3.fromRGB(10,10,14)
    }
    local LIGHT = {
        bg = Color3.fromRGB(248,248,250), sidebar = Color3.fromRGB(255,255,255),
        card = Color3.fromRGB(255,255,255), border = Color3.fromRGB(218,218,228),
        text = Color3.fromRGB(18,18,22), muted = Color3.fromRGB(145,145,160),
        dim = Color3.fromRGB(235,235,242), sep = Color3.fromRGB(225,225,232),
        header = Color3.fromRGB(252,252,255), inputBg = Color3.fromRGB(242,242,248)
    }
    
    local accent = Color3.fromRGB(90,90,210)
    local green = Color3.fromRGB(40,200,130)
    local red = Color3.fromRGB(240,80,100)
    local gold = Color3.fromRGB(241,196,15)
    local purple = Color3.fromRGB(160,100,240)
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
    -- WINDOW SIZING
    -- ==========================================
    local WIN_W, WIN_H, SIDE_W
    if isMobile then
        WIN_W, WIN_H, SIDE_W = 280, 450, 70
    else
        WIN_W, WIN_H, SIDE_W = 440, 480, 120
    end
    
    local HUB_POS = UDim2.new(0.02, 0, 0.5, -WIN_H/2)
    
    -- ==========================================
    -- UTILITY FUNCTIONS
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
        reg(t1,"TextColor3",LIGHT.text,DARK.text)
        
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
        reg(t2,"TextColor3",LIGHT.text,DARK.text)
        
        TweenService:Create(n,tw(),{Size=UDim2.new(0,280,0,58)}):Play()
        
        task.delay(3, function()
            TweenService:Create(n,tw(),{Size=UDim2.new(0,0,0,58),BackgroundTransparency=1}):Play()
            task.wait(0.3)
            if n and n.Parent then n:Destroy() end
        end)
    end
    
    -- ==========================================
    -- COMPONENT BUILDERS
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
    
    local function makeSidebar(win, tabs, onTab)
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
        
        -- PRO pill
        local proPill = Instance.new("Frame")
        proPill.Size = UDim2.new(0,44,0,16)
        proPill.Position = UDim2.new(1,-76,0.5,-8)
        proPill.BackgroundColor3 = isDark and Color3.fromRGB(30,30,44) or Color3.fromRGB(235,235,248)
        proPill.BorderSizePixel = 0
        proPill.ZIndex = 5
        proPill.Parent = hdr
        Instance.new("UICorner", proPill).CornerRadius = UDim.new(1,0)
        
        local proTxt = Instance.new("TextLabel")
        proTxt.Size = UDim2.new(1,0,1,0)
        proTxt.Text = "PRO"
        proTxt.Font = Enum.Font.GothamBold
        proTxt.TextSize = 7
        proTxt.TextColor3 = accent
        proTxt.BackgroundTransparency = 1
        proTxt.ZIndex = 6
        proTxt.Parent = proPill
        
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
        
        closeBtn.MouseButton1Click:Connect(function() sg:Destroy() end)
        closeBtn.TouchTap:Connect(function() sg:Destroy() end)
        
        return hdr, title, closeBtn
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
        
        local padding = Instance.new("UIPadding")
        padding.PaddingTop = UDim.new(0,10)
        padding.PaddingBottom = UDim.new(0,10)
        padding.PaddingLeft = UDim.new(0,8)
        padding.PaddingRight = UDim.new(0,8)
        padding.Parent = pg
        
        layout:GetPropertyChangedSignal("AbsoluteContentSize"):Connect(function()
            pg.CanvasSize = UDim2.new(0,0,0,layout.AbsoluteContentSize.Y+20)
        end)
        
        return pg
    end
    
    local function makeCard(parent, order)
        local card = Instance.new("Frame")
        card.Size = UDim2.new(1,0,0,0)
        card.AutomaticSize = Enum.AutomaticSize.Y
        card.BackgroundColor3 = T.card
        card.BorderSizePixel = 0
        card.LayoutOrder = order
        card.ZIndex = 3
        card.Parent = parent
        Instance.new("UICorner", card).CornerRadius = UDim.new(0,10)
        
        local s = Instance.new("UIStroke")
        s.Color = T.border
        s.Thickness = 1
        s.Parent = card
        reg(card,"BackgroundColor3",LIGHT.card,DARK.card)
        reg(s,"Color",LIGHT.border,DARK.border)
        
        local l = Instance.new("UIListLayout")
        l.SortOrder = Enum.SortOrder.LayoutOrder
        l.HorizontalAlignment = Enum.HorizontalAlignment.Center
        l.Padding = UDim.new(0,0)
        l.Parent = card
        return card, s
    end
    
    local function makeSection(parent, title, color, order)
        local lbl = Instance.new("TextLabel")
        lbl.Size = UDim2.new(1,0,0,20)
        lbl.Text = string.upper(title)
        lbl.Font = Enum.Font.GothamBold
        lbl.TextSize = 8
        lbl.TextColor3 = color
        lbl.TextXAlignment = Enum.TextXAlignment.Left
        lbl.BackgroundTransparency = 1
        lbl.LayoutOrder = order
        lbl.ZIndex = 3
        lbl.Parent = parent
        
        local line = Instance.new("Frame")
        line.Size = UDim2.new(0,24,0,2)
        line.Position = UDim2.new(0,0,1,-2)
        line.BackgroundColor3 = color
        line.BorderSizePixel = 0
        line.ZIndex = 3
        line.Parent = lbl
        Instance.new("UICorner", line).CornerRadius = UDim.new(1,0)
        return lbl
    end
    
    local function makeRow(card, rowOrder, isLast, h)
        local row = Instance.new("Frame")
        row.Size = UDim2.new(1,0,0,h or 42)
        row.BackgroundTransparency = 1
        row.BorderSizePixel = 0
        row.LayoutOrder = rowOrder
        row.ZIndex = 4
        row.Parent = card
        
        if not isLast then
            local sep = Instance.new("Frame")
            sep.Size = UDim2.new(1,-24,0,1)
            sep.Position = UDim2.new(0,12,1,-1)
            sep.BackgroundColor3 = T.sep
            sep.BorderSizePixel = 0
            sep.ZIndex = 4
            sep.Parent = row
            reg(sep,"BackgroundColor3",LIGHT.sep,DARK.sep)
        end
        return row
    end
    
    local function makeScriptRow(parent, scriptData, order, isPremium, onLaunch)
        local col = isPremium and purple or green
        local row = Instance.new("TextButton")
        row.Size = UDim2.new(1,0,0,58)
        row.BackgroundColor3 = T.card
        row.BorderSizePixel = 0
        row.Text = ""
        row.AutoButtonColor = false
        row.LayoutOrder = order
        row.ZIndex = 3
        row.Parent = parent
        Instance.new("UICorner", row).CornerRadius = UDim.new(0,9)
        
        local rs = Instance.new("UIStroke")
        rs.Color = T.border
        rs.Thickness = 1
        rs.Parent = row
        reg(row,"BackgroundColor3",LIGHT.card,DARK.card)
        reg(rs,"Color",LIGHT.border,DARK.border)
        
        local strip = Instance.new("Frame")
        strip.Size = UDim2.new(0,3,0.65,0)
        strip.Position = UDim2.new(0,0,0.175,0)
        strip.BackgroundColor3 = col
        strip.BorderSizePixel = 0
        strip.Parent = row
        Instance.new("UICorner", strip).CornerRadius = UDim.new(0,4)
        
        local ibg = Instance.new("Frame")
        ibg.Size = UDim2.new(0,36,0,36)
        ibg.Position = UDim2.new(0,12,0.5,-18)
        ibg.BackgroundColor3 = col
        ibg.BackgroundTransparency = 0.82
        ibg.BorderSizePixel = 0
        ibg.Parent = row
        Instance.new("UICorner", ibg).CornerRadius = UDim.new(0,8)
        
        local ico = Instance.new("TextLabel")
        ico.Size = UDim2.new(1,0,1,0)
        ico.Text = scriptData.icon or "📦"
        ico.TextSize = 20
        ico.BackgroundTransparency = 1
        ico.Parent = ibg
        
        local nameLbl = Instance.new("TextLabel")
        nameLbl.Size = UDim2.new(1,-120,0,18)
        nameLbl.Position = UDim2.new(0,56,0,8)
        nameLbl.Text = scriptData.name
        nameLbl.Font = Enum.Font.GothamSemibold
        nameLbl.TextSize = isMobile and 13 or 12
        nameLbl.TextColor3 = T.text
        nameLbl.TextXAlignment = Enum.TextXAlignment.Left
        nameLbl.BackgroundTransparency = 1
        nameLbl.ZIndex = 4
        nameLbl.Parent = row
        reg(nameLbl,"TextColor3",LIGHT.text,DARK.text)
        
        local descLbl = Instance.new("TextLabel")
        descLbl.Size = UDim2.new(1,-120,0,14)
        descLbl.Position = UDim2.new(0,56,0,30)
        descLbl.Text = scriptData.desc
        descLbl.Font = Enum.Font.Gotham
        descLbl.TextSize = isMobile and 11 or 10
        descLbl.TextColor3 = T.muted
        descLbl.TextXAlignment = Enum.TextXAlignment.Left
        descLbl.BackgroundTransparency = 1
        descLbl.ZIndex = 4
        descLbl.Parent = row
        reg(descLbl,"TextColor3",LIGHT.muted,DARK.muted)
        
        local badge = Instance.new("TextLabel")
        badge.Size = UDim2.new(0,68,0,20)
        badge.Position = UDim2.new(1,-76,0.5,-10)
        badge.Text = scriptData.tag
        badge.Font = Enum.Font.GothamBold
        badge.TextSize = 9
        badge.TextColor3 = Color3.new(1,1,1)
        badge.BackgroundColor3 = col
        badge.BorderSizePixel = 0
        badge.ZIndex = 4
        badge.Parent = row
        Instance.new("UICorner", badge).CornerRadius = UDim.new(0,5)
        
        row.MouseButton1Click:Connect(onLaunch)
        row.TouchTap:Connect(onLaunch)
        return row
    end
    
    -- ==========================================
    -- EXECUTOR DETECTION
    -- ==========================================
    local function getExecutor()
        local executor = "Unknown"
        local executorInfo = {
            name = "Unknown",
            version = "N/A",
            color = Color3.fromRGB(150,150,150)
        }
        
        pcall(function()
            if identifyexecutor then
                local result = identifyexecutor()
                if result and type(result)=="string" then executor = result end
            end
        end)
        
        if executor == "Unknown" then
            local executors = {
                {name="Synapse", color=Color3.fromRGB(255,70,70), check=function() return syn and syn.request end},
                {name="Krnl", color=Color3.fromRGB(255,215,0), check=function() return krnl and krnl.loadstring end},
                {name="Fluxus", color=Color3.fromRGB(0,200,100), check=function() return fluxus and fluxus.request end},
            }
            for _, exe in ipairs(executors) do
                pcall(function()
                    if exe.check() then
                        executor = exe.name
                        executorInfo.color = exe.color
                    end
                end)
                if executor ~= "Unknown" then break end
            end
        end
        
        executorInfo.name = executor
        return executorInfo
    end
    
    -- ==========================================
    -- FPS UNLOCK
    -- ==========================================
    local fpsUnlocked = false
    local currentFPS = 60
    
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
    -- BUILD HUB WINDOW
    -- ==========================================
    local hubWin, hubStroke = makeWin(HUB_POS)
    
    local HUB_TABS = {
        {name="HOME", icon="🏠", color=accent},
        {name="FREE", icon="📦", color=green},
        {name="PRIVATE", icon="🔒", color=purple},
    }
    
    local hubPages = {}
    local hubNavBtns = {}
    local hubActiveTab = 1
    
    local function setHubTab(idx)
        hubActiveTab = idx
        for i,btn in ipairs(hubNavBtns) do
            local on = (i==idx)
            TweenService:Create(btn.bg,tw(0.15),{BackgroundColor3=on and btn.color or T.dim, BackgroundTransparency=on and 0.15 or 0}):Play()
            TweenService:Create(btn.lbl,tw(0.15),{TextColor3=on and btn.color or T.muted}):Play()
            TweenService:Create(btn.ico,tw(0.15),{TextTransparency=on and 0 or 0.35}):Play()
            btn.bar.BackgroundTransparency = on and 0 or 1
            hubPages[i].Visible = on
        end
    end
    
    local hubSB, hubNB = makeSidebar(hubWin, HUB_TABS, setHubTab)
    hubNavBtns = hubNB
    
    local hubHdr, hubTitle, hubClose = makeHeader(hubWin, "Feather Hub")
    makeDrag(hubWin, hubHdr)
    
    local hubContent = makeContent(hubWin)
    for i,def in ipairs(HUB_TABS) do
        local pg = makePage(hubContent, def.color)
        pg.Visible = (i==1)
        hubPages[i] = pg
    end
    
    -- ==========================================
    -- PAGE 1: HOME (Key System)
    -- ==========================================
    local hp1 = hubPages[1]
    
    -- Welcome card
    local welcomeCard, _ = makeCard(hp1, 1)
    local wcRow = makeRow(welcomeCard, 1, true, isMobile and 64 or 56)
    
    local wIconBg = Instance.new("Frame")
    wIconBg.Size = UDim2.new(0,36,0,36)
    wIconBg.Position = UDim2.new(0,12,0.5,-18)
    wIconBg.BackgroundColor3 = accent
    wIconBg.BackgroundTransparency = 0.85
    wIconBg.BorderSizePixel = 0
    wIconBg.Parent = wcRow
    Instance.new("UICorner", wIconBg).CornerRadius = UDim.new(0,8)
    
    local wIcon = Instance.new("TextLabel")
    wIcon.Size = UDim2.new(1,0,1,0)
    wIcon.Text = "👋"
    wIcon.TextSize = 20
    wIcon.BackgroundTransparency = 1
    wIcon.Parent = wIconBg
    
    local wTitle = Instance.new("TextLabel")
    wTitle.Size = UDim2.new(1,-60,0,18)
    wTitle.Position = UDim2.new(0,56,0,isMobile and 10 or 8)
    wTitle.Text = "Welcome, "..DisplayName
    wTitle.Font = Enum.Font.GothamBold
    wTitle.TextSize = isMobile and 13 or 12
    wTitle.TextColor3 = T.text
    wTitle.TextXAlignment = Enum.TextXAlignment.Left
    wTitle.BackgroundTransparency = 1
    wTitle.ZIndex = 5
    wTitle.Parent = wcRow
    reg(wTitle,"TextColor3",LIGHT.text,DARK.text)
    
    local wSub = Instance.new("TextLabel")
    wSub.Size = UDim2.new(1,-60,0,14)
    wSub.Position = UDim2.new(0,56,0,isMobile and 28 or 26)
    wSub.Text = "Use the sidebar to navigate"
    wSub.Font = Enum.Font.Gotham
    wSub.TextSize = isMobile and 10 or 9
    wSub.TextColor3 = T.muted
    wSub.TextXAlignment = Enum.TextXAlignment.Left
    wSub.BackgroundTransparency = 1
    wSub.ZIndex = 5
    wSub.Parent = wcRow
    reg(wSub,"TextColor3",LIGHT.muted,DARK.muted)
    
    -- Key entry card
    makeSection(hp1, "Private Access", purple, 2)
    local keyCard, _ = makeCard(hp1, 3)
    
    local keyTopRow = makeRow(keyCard, 1, false, 44)
    
    local keyIconBg = Instance.new("Frame")
    keyIconBg.Size = UDim2.new(0,30,0,30)
    keyIconBg.Position = UDim2.new(0,12,0.5,-15)
    keyIconBg.BackgroundColor3 = gold
    keyIconBg.BackgroundTransparency = 0.82
    keyIconBg.BorderSizePixel = 0
    keyIconBg.Parent = keyTopRow
    Instance.new("UICorner", keyIconBg).CornerRadius = UDim.new(0,7)
    
    local keyIcon = Instance.new("TextLabel")
    keyIcon.Size = UDim2.new(1,0,1,0)
    keyIcon.Text = "🔑"
    keyIcon.TextSize = 16
    keyIcon.BackgroundTransparency = 1
    keyIcon.Parent = keyIconBg
    
    local keyTitleLbl = Instance.new("TextLabel")
    keyTitleLbl.Size = UDim2.new(1,-54,0,16)
    keyTitleLbl.Position = UDim2.new(0,50,0,8)
    keyTitleLbl.Text = "Enter Private Key"
    keyTitleLbl.Font = Enum.Font.GothamSemibold
    keyTitleLbl.TextSize = isMobile and 12 or 11
    keyTitleLbl.TextColor3 = T.text
    keyTitleLbl.TextXAlignment = Enum.TextXAlignment.Left
    keyTitleLbl.BackgroundTransparency = 1
    keyTitleLbl.ZIndex = 5
    keyTitleLbl.Parent = keyTopRow
    reg(keyTitleLbl,"TextColor3",LIGHT.text,DARK.text)
    
    local keySubLbl = Instance.new("TextLabel")
    keySubLbl.Size = UDim2.new(1,-54,0,12)
    keySubLbl.Position = UDim2.new(0,50,0,24)
    keySubLbl.Text = "Unlocks all private scripts"
    keySubLbl.Font = Enum.Font.Gotham
    keySubLbl.TextSize = isMobile and 9 or 8
    keySubLbl.TextColor3 = T.muted
    keySubLbl.TextXAlignment = Enum.TextXAlignment.Left
    keySubLbl.BackgroundTransparency = 1
    keySubLbl.ZIndex = 5
    keySubLbl.Parent = keyTopRow
    reg(keySubLbl,"TextColor3",LIGHT.muted,DARK.muted)
    
    local keyInputRow = makeRow(keyCard, 2, false, 44)
    
    local keyInput = Instance.new("TextBox")
    keyInput.Size = UDim2.new(1,-24,0,28)
    keyInput.Position = UDim2.new(0,12,0.5,-14)
    keyInput.PlaceholderText = "Enter your key..."
    keyInput.Text = ""
    keyInput.Font = Enum.Font.GothamMedium
    keyInput.TextSize = isMobile and 12 or 10
    keyInput.TextColor3 = T.text
    keyInput.PlaceholderColor3 = T.muted
    keyInput.BackgroundColor3 = T.inputBg
    keyInput.BorderSizePixel = 0
    keyInput.ClearTextOnFocus = false
    keyInput.ZIndex = 5
    keyInput.Parent = keyInputRow
    Instance.new("UICorner", keyInput).CornerRadius = UDim.new(0,7)
    
    local kiS = Instance.new("UIStroke")
    kiS.Color = T.border
    kiS.Thickness = 1
    kiS.Parent = keyInput
    reg(keyInput,"BackgroundColor3",LIGHT.inputBg,DARK.inputBg)
    reg(keyInput,"TextColor3",LIGHT.text,DARK.text)
    reg(kiS,"Color",LIGHT.border,DARK.border)
    
    local keyBtnRow = makeRow(keyCard, 3, true, 44)
    
    local verifyBtn = Instance.new("TextButton")
    verifyBtn.Size = UDim2.new(0.5,-14,0,28)
    verifyBtn.Position = UDim2.new(0,12,0.5,-14)
    verifyBtn.Text = "Verify Key"
    verifyBtn.Font = Enum.Font.GothamBold
    verifyBtn.TextSize = isMobile and 12 or 10
    verifyBtn.TextColor3 = Color3.new(1,1,1)
    verifyBtn.BackgroundColor3 = accent
    verifyBtn.BorderSizePixel = 0
    verifyBtn.AutoButtonColor = false
    verifyBtn.ZIndex = 5
    verifyBtn.Parent = keyBtnRow
    Instance.new("UICorner", verifyBtn).CornerRadius = UDim.new(0,7)
    
    local getKeyBtn = Instance.new("TextButton")
    getKeyBtn.Size = UDim2.new(0.5,-14,0,28)
    getKeyBtn.Position = UDim2.new(0.5,2,0.5,-14)
    getKeyBtn.Text = "🔗  Get Key"
    getKeyBtn.Font = Enum.Font.GothamBold
    getKeyBtn.TextSize = isMobile and 12 or 10
    getKeyBtn.TextColor3 = T.muted
    getKeyBtn.BackgroundColor3 = T.dim
    getKeyBtn.BorderSizePixel = 0
    getKeyBtn.AutoButtonColor = false
    getKeyBtn.ZIndex = 5
    getKeyBtn.Parent = keyBtnRow
    Instance.new("UICorner", getKeyBtn).CornerRadius = UDim.new(0,7)
    reg(getKeyBtn,"BackgroundColor3",LIGHT.dim,DARK.dim)
    reg(getKeyBtn,"TextColor3",LIGHT.muted,DARK.muted)
    
    getKeyBtn.MouseButton1Click:Connect(function()
        pcall(function() if setclipboard then setclipboard(DISCORD_INVITE) end end)
        notify("Discord Copied!", "Join to get your private key", "success")
    end)
    
    -- Status card
    makeSection(hp1, "Hub Status", accent, 4)
    local statusCard, _ = makeCard(hp1, 5)
    
    local sRow1 = makeRow(statusCard, 1, false, 36)
    local sL1 = Instance.new("TextLabel")
    sL1.Size = UDim2.new(0.5,0,1,0)
    sL1.Position = UDim2.new(0,14,0,0)
    sL1.Text = "Free Scripts"
    sL1.Font = Enum.Font.GothamSemibold
    sL1.TextSize = isMobile and 11 or 10
    sL1.TextColor3 = T.muted
    sL1.TextXAlignment = Enum.TextXAlignment.Left
    sL1.BackgroundTransparency = 1
    sL1.ZIndex = 5
    sL1.Parent = sRow1
    reg(sL1,"TextColor3",LIGHT.muted,DARK.muted)
    
    local sR1 = Instance.new("TextLabel")
    sR1.Size = UDim2.new(0.5,0,1,0)
    sR1.Position = UDim2.new(0.5,0,0,0)
    sR1.Text = tostring(#FREE_SCRIPTS).." available"
    sR1.Font = Enum.Font.GothamBold
    sR1.TextSize = isMobile and 11 or 10
    sR1.TextColor3 = green
    sR1.TextXAlignment = Enum.TextXAlignment.Right
    sR1.BackgroundTransparency = 1
    sR1.ZIndex = 5
    sR1.Parent = sRow1
    
    local sRow2 = makeRow(statusCard, 2, true, 36)
    local sL2 = Instance.new("TextLabel")
    sL2.Size = UDim2.new(0.5,0,1,0)
    sL2.Position = UDim2.new(0,14,0,0)
    sL2.Text = "Private Scripts"
    sL2.Font = Enum.Font.GothamSemibold
    sL2.TextSize = isMobile and 11 or 10
    sL2.TextColor3 = T.muted
    sL2.TextXAlignment = Enum.TextXAlignment.Left
    sL2.BackgroundTransparency = 1
    sL2.ZIndex = 5
    sL2.Parent = sRow2
    reg(sL2,"TextColor3",LIGHT.muted,DARK.muted)
    
    local sR2 = Instance.new("TextLabel")
    sR2.Size = UDim2.new(0.5,0,1,0)
    sR2.Position = UDim2.new(0.5,0,0,0)
    sR2.Text = tostring(#PREMIUM_SCRIPTS).." locked"
    sR2.Font = Enum.Font.GothamBold
    sR2.TextSize = isMobile and 11 or 10
    sR2.TextColor3 = gold
    sR2.TextXAlignment = Enum.TextXAlignment.Right
    sR2.BackgroundTransparency = 1
    sR2.ZIndex = 5
    sR2.Parent = sRow2
    
    -- Key verification logic
    local keyVerified = false
    verifyBtn.MouseButton1Click:Connect(function()
        local entered = keyInput.Text
        if entered=="" then notify("No key entered","Type your key first","error") return end
        notify("Verifying...","Checking your key","warning")
        TweenService:Create(verifyBtn,tw(),{BackgroundColor3=gold}):Play()
        task.wait(0.8)
        
        if VALID_KEYS[entered] then
            keyVerified = true
            TweenService:Create(verifyBtn,tw(),{BackgroundColor3=green}):Play()
            verifyBtn.Text="✓  Verified"
            sR2.Text = tostring(#PREMIUM_SCRIPTS).." unlocked"
            sR2.TextColor3 = green
            notify("Key Valid!","Private scripts are now unlocked","success")
            task.wait(0.5)
            setHubTab(3)
        else
            TweenService:Create(verifyBtn,tw(),{BackgroundColor3=red}):Play()
            notify("Invalid Key","Get a key from our Discord","error")
        end
    end)
    
    -- ==========================================
    -- PAGE 2: FREE SCRIPTS
    -- ==========================================
    local hp2 = hubPages[2]
    
    local freeHeader = Instance.new("TextLabel")
    freeHeader.Size = UDim2.new(1,0,0,20)
    freeHeader.LayoutOrder = 1
    freeHeader.Text = "📋 Available Free Scripts ("..#FREE_SCRIPTS..")"
    freeHeader.Font = Enum.Font.GothamBold
    freeHeader.TextSize = isMobile and 12 or 11
    freeHeader.TextColor3 = T.text
    freeHeader.BackgroundTransparency = 1
    freeHeader.TextXAlignment = Enum.TextXAlignment.Left
    freeHeader.BorderSizePixel = 0
    freeHeader.ZIndex = 3
    freeHeader.Parent = hp2
    reg(freeHeader,"TextColor3",LIGHT.text,DARK.text)
    
    for i, s in ipairs(FREE_SCRIPTS) do
        makeScriptRow(hp2, s, i+1, false, function()
            if s.url=="" then notify("Coming Soon",s.name.." is in development","warning") return end
            notify("Launching...",s.name,"success")
            task.spawn(function()
                local ok,err=pcall(function() loadstring(game:HttpGet(s.url))() end)
                if not ok then notify("Error","Failed: "..tostring(err):sub(1,50),"error") end
            end)
        end)
    end
    
    -- ==========================================
    -- PAGE 3: PRIVATE SCRIPTS
    -- ==========================================
    local hp3 = hubPages[3]
    
    local lockNotice = Instance.new("Frame")
    lockNotice.Size = UDim2.new(1,0,0,0)
    lockNotice.AutomaticSize = Enum.AutomaticSize.Y
    lockNotice.BackgroundColor3 = T.card
    lockNotice.BorderSizePixel = 0
    lockNotice.LayoutOrder = 1
    lockNotice.ZIndex = 3
    lockNotice.Parent = hp3
    Instance.new("UICorner", lockNotice).CornerRadius = UDim.new(0,10)
    
    local lnS = Instance.new("UIStroke")
    lnS.Color = T.border
    lnS.Thickness = 1
    lnS.Parent = lockNotice
    reg(lockNotice,"BackgroundColor3",LIGHT.card,DARK.card)
    reg(lnS,"Color",LIGHT.border,DARK.border)
    
    local lnRow = makeRow(lockNotice, 1, true, 58)
    
    local lnIcon = Instance.new("TextLabel")
    lnIcon.Size = UDim2.new(0,36,0,36)
    lnIcon.Position = UDim2.new(0,12,0.5,-18)
    lnIcon.Text = "🔒"
    lnIcon.TextSize = 24
    lnIcon.BackgroundTransparency = 1
    lnIcon.ZIndex = 5
    lnIcon.Parent = lnRow
    
    local lnTxt = Instance.new("TextLabel")
    lnTxt.Size = UDim2.new(1,-100,0,16)
    lnTxt.Position = UDim2.new(0,56,0,8)
    lnTxt.Text = "Private Scripts Locked"
    lnTxt.Font = Enum.Font.GothamBold
    lnTxt.TextSize = isMobile and 14 or 13
    lnTxt.TextColor3 = T.text
    lnTxt.TextXAlignment = Enum.TextXAlignment.Left
    lnTxt.BackgroundTransparency = 1
    lnTxt.ZIndex = 5
    lnTxt.Parent = lnRow
    reg(lnTxt,"TextColor3",LIGHT.text,DARK.text)
    
    local lnSub = Instance.new("TextLabel")
    lnSub.Size = UDim2.new(1,-100,0,14)
    lnSub.Position = UDim2.new(0,56,0,28)
    lnSub.Text = "Enter your key on the Home tab"
    lnSub.Font = Enum.Font.Gotham
    lnSub.TextSize = isMobile and 11 or 10
    lnSub.TextColor3 = T.muted
    lnSub.TextXAlignment = Enum.TextXAlignment.Left
    lnSub.BackgroundTransparency = 1
    lnSub.ZIndex = 5
    lnSub.Parent = lnRow
    reg(lnSub,"TextColor3",LIGHT.muted,DARK.muted)
    
    local goHomeBtn = Instance.new("TextButton")
    goHomeBtn.Size = UDim2.new(0,80,0,28)
    goHomeBtn.Position = UDim2.new(1,-90,0.5,-14)
    goHomeBtn.Text = "Go Home"
    goHomeBtn.Font = Enum.Font.GothamBold
    goHomeBtn.TextSize = isMobile and 12 or 11
    goHomeBtn.TextColor3 = Color3.new(1,1,1)
    goHomeBtn.BackgroundColor3 = accent
    goHomeBtn.BorderSizePixel = 0
    goHomeBtn.AutoButtonColor = false
    goHomeBtn.ZIndex = 5
    goHomeBtn.Parent = lnRow
    Instance.new("UICorner", goHomeBtn).CornerRadius = UDim.new(0,7)
    goHomeBtn.MouseButton1Click:Connect(function() setHubTab(1) end)
    
    local premiumRows = {}
    for i, s in ipairs(PREMIUM_SCRIPTS) do
        local row = makeScriptRow(hp3, s, i+1, true, function()
            if not keyVerified then notify("Key Required","Verify your key on the Home tab","error") return end
            if s.url=="" then notify("Coming Soon",s.name.." is in development","warning") return end
            notify("Launching...",s.name,"success")
            task.spawn(function()
                local ok,err=pcall(function() loadstring(game:HttpGet(s.url))() end)
                if not ok then notify("Error","Failed: "..tostring(err):sub(1,50),"error") end
            end)
        end)
        row.Visible = false
        table.insert(premiumRows, row)
    end
    
    local function unlockPremium()
        lockNotice.Visible = false
        for _, r in ipairs(premiumRows) do r.Visible = true end
    end
    
    verifyBtn.MouseButton1Click:Connect(function()
        if VALID_KEYS[keyInput.Text] then
            task.wait(1.5)
            unlockPremium()
        end
    end)
    
    -- ==========================================
    -- SIDEBAR BOTTOM BUTTONS
    -- ==========================================
    local bottomContainer = Instance.new("Frame")
    bottomContainer.Size = UDim2.new(1,-12,0,90)
    bottomContainer.Position = UDim2.new(0,6,1,-95)
    bottomContainer.BackgroundTransparency = 1
    bottomContainer.ZIndex = 5
    bottomContainer.Parent = hubSB
    
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
    -- INITIALIZE
    -- ==========================================
    setHubTab(1)
    
    -- Hide keybind
    local hideKey = Enum.KeyCode.RightBracket
    UserInputService.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.Keyboard and input.KeyCode == hideKey then
            sg.Enabled = not sg.Enabled
        end
    end)
    
    -- Open animation
    hubWin.Size = UDim2.new(0,0,0,0)
    hubWin.BackgroundTransparency = 1
    TweenService:Create(hubWin, tw(0.38,Enum.EasingStyle.Back,Enum.EasingDirection.Out), {Size=UDim2.new(0,WIN_W,0,WIN_H), BackgroundTransparency=0}):Play()
    
    notify("Key System", "Loaded successfully", "success")
    print("[Feather Hub] Key System loaded - ] to toggle")
end
