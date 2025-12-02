# ALUGenratortrading-V2.0
Alu generator trading membantu kita untuk trading nyaman dan aman , dapat menetukan entry optimal dengan reason yang logis, menentukan arahmarket dengan basis scalping/intraday/swing 
<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ALUGenratortrading V2.0 - AI Trading Generator</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <script src="https://unpkg.com/lightweight-charts/dist/lightweight-charts.standalone.production.js"></script>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        :root {
            --primary: #3a86ff;
            --secondary: #8338ec;
            --success: #38b000;
            --danger: #ff006e;
            --warning: #ffbe0b;
            --dark: #0f172a;
            --light: #f8fafc;
            --card-bg: #1e293b;
            --border: #334155;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #0f172a 0%, #1e293b 100%);
            color: var(--light);
            min-height: 100vh;
            padding: 20px;
        }

        .container {
            max-width: 1600px;
            margin: 0 auto;
        }

        /* Header */
        .header {
            text-align: center;
            margin-bottom: 30px;
            padding: 20px;
            background: linear-gradient(90deg, var(--primary), var(--secondary));
            border-radius: 15px;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.3);
        }

        .logo {
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 15px;
            margin-bottom: 10px;
        }

        .logo i {
            font-size: 2.5rem;
            color: var(--warning);
        }

        .logo h1 {
            font-size: 2.8rem;
            background: linear-gradient(90deg, #fff, var(--warning));
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            text-shadow: 0 2px 10px rgba(0, 0, 0, 0.3);
        }

        .version {
            display: inline-block;
            background: var(--warning);
            color: var(--dark);
            padding: 3px 10px;
            border-radius: 20px;
            font-size: 0.8rem;
            font-weight: bold;
            margin-left: 10px;
        }

        /* Control Panel */
        .control-panel {
            background: var(--card-bg);
            border-radius: 15px;
            padding: 25px;
            margin-bottom: 30px;
            border: 1px solid var(--border);
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
            gap: 20px;
        }

        .control-group {
            display: flex;
            flex-direction: column;
            gap: 10px;
        }

        label {
            font-weight: 600;
            color: var(--warning);
            font-size: 0.9rem;
            text-transform: uppercase;
            letter-spacing: 1px;
        }

        select, .btn {
            padding: 12px 20px;
            border-radius: 8px;
            border: 2px solid var(--border);
            background: #2d3748;
            color: var(--light);
            font-size: 1rem;
            cursor: pointer;
            transition: all 0.3s ease;
        }

        select:focus, .btn:hover {
            outline: none;
            border-color: var(--primary);
            box-shadow: 0 0 0 3px rgba(58, 134, 255, 0.2);
        }

        .btn-generate {
            background: linear-gradient(90deg, var(--primary), var(--secondary));
            border: none;
            font-weight: bold;
            font-size: 1.1rem;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 10px;
        }

        .btn-generate:hover {
            transform: translateY(-2px);
            box-shadow: 0 5px 20px rgba(58, 134, 255, 0.4);
        }

        .timeframe-group {
            display: flex;
            flex-wrap: wrap;
            gap: 8px;
        }

        .tf-btn {
            flex: 1;
            min-width: 60px;
            padding: 10px;
            text-align: center;
            background: #374151;
            border: 2px solid transparent;
            border-radius: 6px;
            cursor: pointer;
            transition: all 0.2s;
        }

        .tf-btn.active {
            background: var(--primary);
            border-color: var(--primary);
            font-weight: bold;
        }

        /* Main Content */
        .main-content {
            display: grid;
            grid-template-columns: 2fr 1fr;
            gap: 30px;
            margin-bottom: 30px;
        }

        @media (max-width: 1200px) {
            .main-content {
                grid-template-columns: 1fr;
            }
        }

        /* Chart Container */
        .chart-container {
            background: var(--card-bg);
            border-radius: 15px;
            padding: 20px;
            border: 1px solid var(--border);
            height: 600px;
            position: relative;
        }

        .chart-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 20px;
            padding-bottom: 15px;
            border-bottom: 1px solid var(--border);
        }

        #chart {
            width: 100%;
            height: calc(100% - 60px);
        }

        /* Analysis Panel */
        .analysis-panel {
            background: var(--card-bg);
            border-radius: 15px;
            padding: 25px;
            border: 1px solid var(--border);
            overflow-y: auto;
            max-height: 600px;
        }

        .signal-badge {
            display: inline-block;
            padding: 12px 20px;
            border-radius: 10px;
            font-weight: bold;
            margin-bottom: 20px;
            text-align: center;
            width: 100%;
            font-size: 1.1rem;
        }

        .signal-badge.buy {
            background: linear-gradient(90deg, #065f46, #10b981);
            border-left: 5px solid #10b981;
        }

        .signal-badge.sell {
            background: linear-gradient(90deg, #7f1d1d, #ef4444);
            border-left: 5px solid #ef4444;
        }

        /* Reason for Entry Section */
        .reason-section {
            background: rgba(59, 130, 246, 0.1);
            border-radius: 10px;
            padding: 20px;
            margin-bottom: 25px;
            border-left: 4px solid var(--primary);
        }

        .reason-title {
            color: var(--warning);
            margin-bottom: 10px;
            display: flex;
            align-items: center;
            gap: 10px;
        }

        .reason-content {
            line-height: 1.6;
        }

        .reason-points {
            margin-top: 15px;
            display: grid;
            gap: 8px;
        }

        .reason-point {
            display: flex;
            align-items: flex-start;
            gap: 10px;
            padding: 5px 0;
        }

        .reason-point i {
            color: var(--primary);
            margin-top: 3px;
        }

        .analysis-section {
            margin-bottom: 25px;
        }

        .section-title {
            color: var(--warning);
            margin-bottom: 15px;
            padding-bottom: 10px;
            border-bottom: 2px solid var(--border);
            display: flex;
            align-items: center;
            gap: 10px;
        }

        .entry-levels {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 15px;
        }

        .level-box {
            background: #2d3748;
            padding: 15px;
            border-radius: 10px;
            border-left: 4px solid var(--primary);
            transition: transform 0.3s ease;
        }

        .level-box:hover {
            transform: translateY(-3px);
        }

        .level-box.sl {
            border-left-color: var(--danger);
        }

        .level-box.tp {
            border-left-color: var(--success);
        }

        .level-value {
            font-size: 1.3rem;
            font-weight: bold;
            margin-top: 5px;
        }

        /* Visual Entry Guide */
        .visual-guide {
            background: var(--card-bg);
            border-radius: 15px;
            padding: 25px;
            border: 1px solid var(--border);
            margin-bottom: 30px;
        }

        .guide-container {
            display: flex;
            align-items: center;
            justify-content: center;
            height: 200px;
            position: relative;
            background: #1a202c;
            border-radius: 10px;
            overflow: hidden;
            margin-top: 20px;
        }

        .price-level {
            position: absolute;
            left: 20px;
            width: calc(100% - 40px);
            height: 2px;
            display: flex;
            align-items: center;
            justify-content: space-between;
        }

        .price-line {
            flex-grow: 1;
            height: 2px;
        }

        .price-label {
            background: var(--dark);
            padding: 5px 10px;
            border-radius: 4px;
            font-size: 0.9rem;
            min-width: 80px;
            text-align: center;
            margin-left: 10px;
            border: 1px solid var(--border);
        }

        .entry-zone {
            position: absolute;
            left: 20px;
            right: 20px;
            background: rgba(59, 130, 246, 0.3);
            border: 2px dashed var(--primary);
            border-radius: 4px;
        }

        .entry-arrow {
            position: absolute;
            font-size: 2rem;
            color: var(--warning);
            animation: pulse 1.5s infinite;
        }

        @keyframes pulse {
            0% { opacity: 0.5; }
            50% { opacity: 1; }
            100% { opacity: 0.5; }
        }

        /* Footer */
        .footer {
            text-align: center;
            padding: 20px;
            color: #94a3b8;
            font-size: 0.9rem;
            border-top: 1px solid var(--border);
            margin-top: 30px;
        }

        .disclaimer {
            background: rgba(220, 38, 38, 0.1);
            border: 1px solid #dc2626;
            border-radius: 10px;
            padding: 15px;
            margin-top: 20px;
        }

        /* Mobile Responsive */
        @media (max-width: 768px) {
            .control-panel {
                grid-template-columns: 1fr;
            }
            
            .logo h1 {
                font-size: 2rem;
            }
            
            .main-content {
                grid-template-columns: 1fr;
                gap: 20px;
            }
            
            .chart-container {
                height: 400px;
            }
            
            .entry-levels {
                grid-template-columns: 1fr;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <!-- Header -->
        <header class="header">
            <div class="logo">
                <i class="fas fa-robot"></i>
                <div>
                    <h1>ALUGenratortrading <span class="version">V2.0</span></h1>
                    <p class="subtitle">AI Trading Generator with Detailed Entry Reasons</p>
                </div>
            </div>
        </header>

        <!-- Control Panel -->
        <div class="control-panel">
            <div class="control-group">
                <label><i class="fas fa-chart-line"></i> INSTRUMENT</label>
                <select id="instrumentSelect">
                    <option value="EURUSD">EUR/USD (Forex)</option>
                    <option value="XAUUSD">GOLD (XAU/USD)</option>
                    <option value="BTCUSD">BITCOIN (BTC/USD)</option>
                    <option value="ETHUSD">ETHEREUM (ETH/USD)</option>
                    <option value="USOIL">CRUDE OIL (WTI)</option>
                    <option value="GBPUSD">GBP/USD</option>
                    <option value="USDJPY">USD/JPY</option>
                    <option value="AUDUSD">AUD/USD</option>
                </select>
            </div>

            <div class="control-group">
                <label><i class="fas fa-clock"></i> TIMEFRAME</label>
                <div class="timeframe-group">
                    <div class="tf-btn active" data-tf="1m">1M</div>
                    <div class="tf-btn" data-tf="5m">5M</div>
                    <div class="tf-btn" data-tf="15m">15M</div>
                    <div class="tf-btn" data-tf="1h">1H</div>
                    <div class="tf-btn" data-tf="4h">4H</div>
                    <div class="tf-btn" data-tf="D">DAILY</div>
                    <div class="tf-btn" data-tf="W">WEEKLY</div>
                </div>
            </div>

            <div class="control-group">
                <label><i class="fas fa-chess"></i> STRATEGY</label>
                <select id="strategySelect">
                    <option value="SMC">Smart Money Concept (SMC)</option>
                    <option value="ICT">ICT Concepts</option>
                    <option value="HYBRID">Hybrid SMC+ICT</option>
                    <option value="PRICE_ACTION">Price Action</option>
                    <option value="ALL">All Strategies Combined</option>
                </select>
            </div>

            <div class="control-group">
                <label><i class="fas fa-cogs"></i> ACTION</label>
                <button class="btn btn-generate" id="generateBtn">
                    <i class="fas fa-bolt"></i> GENERATE SIGNALS
                </button>
            </div>
        </div>

        <!-- Main Content -->
        <div class="main-content">
            <!-- Chart Section -->
            <div class="chart-container">
                <div class="chart-header">
                    <h3><i class="fas fa-chart-candlestick"></i> LIVE PRICE CHART</h3>
                    <div id="marketStatus" class="market-up">
                        <i class="fas fa-circle"></i> WAITING FOR SIGNAL
                    </div>
                </div>
                <div id="chart"></div>
            </div>

            <!-- Analysis Results -->
            <div class="analysis-panel">
                <div id="signalBadge" class="signal-badge">
                    <i class="fas fa-hourglass-half"></i> WAITING FOR ANALYSIS...
                </div>

                <!-- Reason for Entry Section -->
                <div class="reason-section" id="reasonSection" style="display: none;">
                    <h4 class="reason-title"><i class="fas fa-lightbulb"></i> REASON FOR ENTRY</h4>
                    <div class="reason-content" id="reasonContent">
                        <!-- Reason will be filled by JavaScript -->
                    </div>
                </div>

                <div class="analysis-section">
                    <h4 class="section-title"><i class="fas fa-crosshairs"></i> ENTRY ZONES</h4>
                    <div class="entry-levels">
                        <div class="level-box">
                            <div>OPTIMAL ENTRY</div>
                            <div class="level-value" id="optimalEntry">-</div>
                        </div>
                        <div class="level-box">
                            <div>ENTRY RANGE</div>
                            <div class="level-value" id="entryRange">-</div>
                        </div>
                    </div>
                </div>

                <div class="analysis-section">
                    <h4 class="section-title"><i class="fas fa-shield-alt"></i> RISK MANAGEMENT</h4>
                    <div class="entry-levels">
                        <div class="level-box sl">
                            <div>STOP LOSS</div>
                            <div class="level-value" id="stopLoss">-</div>
                        </div>
                        <div class="level-box tp">
                            <div>TAKE PROFIT 1</div>
                            <div class="level-value" id="takeProfit1">-</div>
                        </div>
                        <div class="level-box tp">
                            <div>TAKE PROFIT 2</div>
                            <div class="level-value" id="takeProfit2">-</div>
                        </div>
                        <div class="level-box tp">
                            <div>MAX TP</div>
                            <div class="level-value" id="maxTP">-</div>
                        </div>
                    </div>
                </div>

                <div class="analysis-section">
                    <h4 class="section-title"><i class="fas fa-chart-bar"></i> ANALYSIS DETAILS</h4>
                    <div id="analysisDetails">
                        <p><strong>Market Condition:</strong> <span id="marketCondition">-</span></p>
                        <p><strong>Strategy Used:</strong> <span id="strategyUsed">-</span></p>
                        <p><strong>Confidence Level:</strong> <span id="confidenceLevel">-</span></p>
                        <p><strong>Risk/Reward:</strong> <span id="riskReward">-</span></p>
                        <p><strong>Expected Duration:</strong> <span id="duration">-</span></p>
                    </div>
                </div>
            </div>
        </div>

        <!-- Visual Entry Guide -->
        <div class="visual-guide">
            <h3 class="section-title"><i class="fas fa-map-signs"></i> VISUAL ENTRY GUIDE</h3>
            <div class="guide-container" id="visualGuide">
                <div class="price-level" style="top: 10%;">
                    <div class="price-line" style="background: #10b981;"></div>
                    <div class="price-label" id="tpMaxLabel">TP MAX: -</div>
                </div>
                <div class="price-level" style="top: 30%;">
                    <div class="price-line" style="background: #10b981;"></div>
                    <div class="price-label" id="tp2Label">TP2: -</div>
                </div>
                <div class="price-level" style="top: 50%;">
                    <div class="price-line" style="background: #10b981;"></div>
                    <div class="price-label" id="tp1Label">TP1: -</div>
                </div>
                <div class="price-level" style="top: 70%;">
                    <div class="price-line" style="background: #3b82f6;"></div>
                    <div class="price-label" id="entryLabel">ENTRY: -</div>
                </div>
                <div class="price-level" style="top: 90%;">
                    <div class="price-line" style="background: #ef4444;"></div>
                    <div class="price-label" id="slLabel">SL: -</div>
                </div>
                
                <div class="entry-zone" id="entryZone" style="display: none;"></div>
                <div class="entry-arrow" id="entryArrow" style="display: none;">
                    <i class="fas fa-arrow-right"></i>
                </div>
            </div>
            <div id="entryInstructions" style="text-align: center; margin-top: 20px; color: #94a3b8;">
                <i class="fas fa-info-circle"></i> Visual guide akan muncul setelah analisis
            </div>
        </div>

        <!-- Footer -->
        <footer class="footer">
            <p>ALUGenratortrading v2.0 • AI Trading Signal Generator with Entry Reasons</p>
            <p><i class="fas fa-sync-alt"></i> Real-time Analysis • <i class="fas fa-lightbulb"></i> Detailed Entry Reasons • <i class="fas fa-robot"></i> AI-Powered</p>
            
            <div class="disclaimer">
                <p><strong><i class="fas fa-exclamation-triangle"></i> DISCLAIMER:</strong> 
                Trading involves substantial risk. This tool is for educational purposes only. Past performance does not guarantee future results. Always test strategies on demo accounts first.</p>
            </div>
            
            <p style="margin-top: 20px;">
                <i class="fas fa-globe"></i> Access: 
                <a href="https://alugenerator-v2.netlify.app" style="color: var(--primary);">Main Site V2</a> • 
                <a href="https://alugenerator-trading.pages.dev" style="color: var(--primary);">Backup Site V1</a>
            </p>
        </footer>
    </div>

    <script>
        // Initialize Chart
        const chartContainer = document.getElementById('chart');
        const chart = LightweightCharts.createChart(chartContainer, {
            width: chartContainer.clientWidth,
            height: 500,
            layout: {
                backgroundColor: '#1e293b',
                textColor: '#d1d5db',
            },
            grid: {
                vertLines: { color: '#2d3748' },
                horzLines: { color: '#2d3748' },
            },
            crosshair: {
                mode: LightweightCharts.CrosshairMode.Normal,
            },
            rightPriceScale: {
                borderColor: '#4b5563',
            },
            timeScale: {
                borderColor: '#4b5563',
                timeVisible: true,
            },
        });

        const candleSeries = chart.addCandlestickSeries({
            upColor: '#10b981',
            downColor: '#ef4444',
            borderDownColor: '#ef4444',
            borderUpColor: '#10b981',
            wickDownColor: '#ef4444',
            wickUpColor: '#10b981',
        });

        // Database of Entry Reasons
        const entryReasons = {
            SMC: {
                BUY: [
                    {
                        title: "BULLISH ORDER BLOCK CONFIRMED",
                        details: "Price has retraced to a valid bullish order block zone with clear market structure shift.",
                        points: [
                            "Fresh bullish OB identified at previous consolidation area",
                            "Liquidity sweep below previous low completed",
                            "Market structure shifted to bullish (HH + HL formed)",
                            "Mitigation block respected with strong rejection candle"
                        ]
                    },
                    {
                        title: "FAIR VALUE GAP FILL WITH REACTION",
                        details: "Price filled a bullish FVG and showed strong reaction with momentum.",
                        points: [
                            "Bullish FVG identified between $HIGH and $LOW",
                            "Price filled 100% of the FVG zone",
                            "Strong bullish reaction with 3 consecutive green candles",
                            "Volume surge detected on entry candle"
                        ]
                    },
                    {
                        title: "BREAKER BLOCK SUPPORT HOLD",
                        details: "Price is holding at breaker block support after breaking previous high.",
                        points: [
                            "Breaker block formed after strong bullish impulse",
                            "Price retraced exactly to breaker block support",
                            "Lower timeframe shows hidden bullish divergence",
                            "No significant sell-side liquidity below current price"
                        ]
                    }
                ],
                SELL: [
                    {
                        title: "BEARISH ORDER BLOCK ACTIVATED",
                        details: "Price rejected from bearish order block with clear distribution pattern.",
                        points: [
                            "Fresh bearish OB confirmed at previous high",
                            "Liquidity sweep above previous high completed",
                            "Market structure shifted to bearish (LH + LL formed)",
                            "Significant sell volume detected at rejection"
                        ]
                    },
                    {
                        title: "BEARISH FVG FILL WITH REJECTION",
                        details: "Price filled bearish FVG and showed immediate rejection.",
                        points: [
                            "Bearish FVG identified at supply zone",
                            "Price filled 85-100% of FVG zone",
                            "Strong rejection with bearish engulfing pattern",
                            "RSI showing overbought divergence"
                        ]
                    }
                ]
            },
            ICT: {
                BUY: [
                    {
                        title: "OPTIMAL TRADE ENTRY (OTE) ACHIEVED",
                        details: "Price reached 62-79% retracement of previous swing with displacement.",
                        points: [
                            "OTE zone identified at 62-79% Fibonacci retracement",
                            "Displacement confirmed on lower timeframe",
                            "Kill zone alignment (Asian/London session)",
                            "Liquidity sweep below OTE completed"
                        ]
                    },
                    {
                        title: "BULLISH MARKET MAKER MODEL",
                        details: "MM executed buy model with liquidity grab and reversal.",
                        points: [
                            "Liquidity sweep below consolidation completed",
                            "Price returned to original consolidation zone",
                            "Buy-side imbalance confirmed on lower TF",
                            "Time-based alignment with session overlap"
                        ]
                    }
                ],
                SELL: [
                    {
                        title: "BEARISH OTE WITH LIQUIDITY SWEEP",
                        details: "Price swept liquidity above OTE zone and showed rejection.",
                        points: [
                            "OTE zone at 62-79% retracement of bearish swing",
                            "Liquidity sweep above OTE completed",
                            "Displacement confirmed with bearish momentum",
                            "New York session kill zone alignment"
                        ]
                    }
                ]
            },
            PRICE_ACTION: {
                BUY: [
                    {
                        title: "BULLISH ENGULFING AT SUPPORT",
                        details: "Strong bullish engulfing pattern formed at major support level.",
                        points: [
                            "Major support level holding for 3+ tests",
                            "Bullish engulfing pattern with volume confirmation",
                            "Higher low established on higher timeframe",
                            "MACD showing bullish crossover on 15M"
                        ]
                    },
                    {
                        title: "PIN BAR REJECTION AT DEMAND ZONE",
                        details: "Pin bar rejection at demand zone with confluence.",
                        points: [
                            "Pin bar with 3:1 body:wick ratio",
                            "Formed at previous demand zone",
                            "Confluence with 200 EMA support",
                            "Stochastic showing oversold reversal"
                        ]
                    }
                ],
                SELL: [
                    {
                        title: "BEARISH ENGULFING AT RESISTANCE",
                        details: "Bearish engulfing pattern at major resistance with volume.",
                        points: [
                            "Major resistance level tested and rejected",
                            "Bearish engulfing with increasing volume",
                            "Lower high confirmed on daily timeframe",
                            "RSI divergence on 4H chart"
                        ]
                    }
                ]
            }
        };

        // Generate sample data
        const generateSampleData = () => {
            const data = [];
            let time = new Date(Date.now() - 1000 * 60 * 60 * 24 * 30);
            let price = 1.0800;
            
            for (let i = 0; i < 500; i++) {
                time = new Date(time.getTime() + 1000 * 60 * 15);
                const open = price;
                const change = (Math.random() - 0.5) * 0.005;
                price = price + change;
                const high = Math.max(open, price) + Math.random() * 0.001;
                const low = Math.min(open, price) - Math.random() * 0.001;
                const close = price;
                
                data.push({
                    time: time.getTime() / 1000,
                    open: open,
                    high: high,
                    low: low,
                    close: close,
                });
            }
            return data;
        };

        candleSeries.setData(generateSampleData());

        // Timeframe buttons
        const timeframeButtons = document.querySelectorAll('.tf-btn');
        timeframeButtons.forEach(btn => {
            btn.addEventListener('click', () => {
                timeframeButtons.forEach(b => b.classList.remove('active'));
                btn.classList.add('active');
            });
        });

        // Generate Button
        document.getElementById('generateBtn').addEventListener('click', generateSignals);

        // Generate Trading Signals with Reasons
        function generateSignals() {
            const instrument = document.getElementById('instrumentSelect').value;
            const timeframe = document.querySelector('.tf-btn.active').dataset.tf;
            const strategy = document.getElementById('strategySelect').value;
            
            // Show loading
            document.getElementById('signalBadge').innerHTML = 
                '<i class="fas fa-cog fa-spin"></i> ANALYZING MARKET...';
            document.getElementById('reasonSection').style.display = 'none';
            
            // Simulate API call delay
            setTimeout(() => {
                // Randomly determine signal direction
                const isBullish = Math.random() > 0.5;
                const signalType = isBullish ? 'BUY' : 'SELL';
                
                // Get appropriate strategy for reasons
                let strategyKey = strategy;
                if (strategy === 'HYBRID' || strategy === 'ALL') {
                    // Randomly pick one for hybrid
                    const strategies = ['SMC', 'ICT', 'PRICE_ACTION'];
                    strategyKey = strategies[Math.floor(Math.random() * strategies.length)];
                }
                
                // Get random reason from database
                const reasons = entryReasons[strategyKey]?.[signalType] || entryReasons.SMC[signalType];
                const selectedReason = reasons[Math.floor(Math.random() * reasons.length)];
                
                // Generate realistic prices
                const basePrices = {
                    'EURUSD': { price: 1.0950, volatility: 0.0020 },
                    'XAUUSD': { price: 1985.50, volatility: 15.0 },
                    'BTCUSD': { price: 42500, volatility: 500 },
                    'ETHUSD': { price: 2250, volatility: 50 },
                    'USOIL': { price: 78.50, volatility: 1.5 },
                    'GBPUSD': { price: 1.2750, volatility: 0.0015 },
                    'USDJPY': { price: 148.20, volatility: 0.30 },
                    'AUDUSD': { price: 0.6580, volatility: 0.0010 },
                };
                
                const instrumentData = basePrices[instrument] || basePrices.EURUSD;
                let currentPrice = instrumentData.price + (Math.random() - 0.5) * instrumentData.volatility;
                
                // Calculate levels
                let entryPrice, slPrice, tp1Price, tp2Price, tpMaxPrice;
                
                if (isBullish) {
                    entryPrice = currentPrice * (1 - 0.0005);
                    slPrice = entryPrice * (1 - 0.0010);
                    tp1Price = entryPrice * (1 + 0.0015);
                    tp2Price = entryPrice * (1 + 0.0030);
                    tpMaxPrice = entryPrice * (1 + 0.0050);
                } else {
                    entryPrice = currentPrice * (1 + 0.0005);
                    slPrice = entryPrice * (1 + 0.0010);
                    tp1Price = entryPrice * (1 - 0.0015);
                    tp2Price = entryPrice * (1 - 0.0030);
                    tpMaxPrice = entryPrice * (1 - 0.0050);
                }
                
                // Format prices
                const formatPrice = (price) => {
                    if (instrument === 'XAUUSD') return price.toFixed(2);
                    if (instrument === 'BTCUSD') return price.toFixed(0);
                    if (instrument === 'ETHUSD') return price.toFixed(1);
                    if (instrument === 'USOIL') return price.toFixed(2);
                    if (instrument === 'USDJPY') return price.toFixed(2);
                    if (instrument === 'EURUSD' || instrument === 'GBPUSD' || instrument === 'AUDUSD') 
                        return price.toFixed(4);
                    return price.toFixed(4);
                };
                
                // Update signal badge
                const badge = document.getElementById('signalBadge');
                badge.innerHTML = `
                    <i class="fas fa-${isBullish ? 'arrow-up' : 'arrow-down'}"></i> 
                    <strong>${signalType} SIGNAL DETECTED</strong><br>
                    <small>${instrument} | ${timeframe.toUpperCase()} | ${strategy}</small>
                `;
                badge.className = `signal-badge ${isBullish ? 'buy' : 'sell'}`;
                
                // Show and update reason section
                const reasonSection = document.getElementById('reasonSection');
                const reasonContent = document.getElementById('reasonContent');
                
                reasonSection.style.display = 'block';
                reasonContent.innerHTML = `
                    <h4 style="color: ${isBullish ? '#10b981' : '#ef4444'}; margin-bottom: 10px;">
                        ${selectedReason.title}
                    </h4>
                    <p style="margin-bottom: 15px; color: #cbd5e1;">${selectedReason.details}</p>
                    <div class="reason-points">
                        ${selectedReason.points.map(point => `
                            <div class="reason-point">
                                <i class="fas fa-check-circle"></i>
                                <span>${point}</span>
                            </div>
                        `).join('')}
                    </div>
                    <div style="margin-top: 15px; padding: 10px; background: rgba(0,0,0,0.2); border-radius: 5px;">
                        <small><i class="fas fa-info-circle"></i> <strong>Confluence:</strong> Multiple timeframe alignment + Volume confirmation + ${strategy} strategy</small>
                    </div>
                `;
                
                // Update entry levels
                document.getElementById('optimalEntry').textContent = formatPrice(entryPrice);
                document.getElementById('entryRange').textContent = 
                    `${formatPrice(entryPrice * (isBullish ? 0.9998 : 1.0002))} - ${formatPrice(entryPrice * (isBullish ? 1.0002 : 0.9998))}`;
                
                // Update risk management
                document.getElementById('stopLoss').textContent = formatPrice(slPrice);
                document.getElementById('takeProfit1').textContent = formatPrice(tp1Price);
                document.getElementById('takeProfit2').textContent = formatPrice(tp2Price);
                document.getElementById('maxTP').textContent = formatPrice(tpMaxPrice);
                
                // Update analysis details
                document.getElementById('marketCondition').textContent = 
                    isBullish ? 'Bullish Trend' : 'Bearish Trend';
                document.getElementById('strategyUsed').textContent = `${strategy} (${strategyKey} applied)`;
                document.getElementById('confidenceLevel').textContent = 
                    `${Math.floor(Math.random() * 30) + 65}%`;
                
                const rrRatio = (Math.abs(tp1Price - entryPrice) / Math.abs(slPrice - entryPrice)).toFixed(2);
                document.getElementById('riskReward').textContent = `1:${rrRatio}`;
                
                document.getElementById('duration').textContent = 
                    timeframe === '1m' ? '5-15 minutes' :
                    timeframe === '5m' ? '15-60 minutes' :
                    timeframe === '15m' ? '1-4 hours' :
                    timeframe === '1h' ? '4-24 hours' :
                    timeframe === '4h' ? '1-3 days' :
                    timeframe === 'D' ? '3-7 days' : '1-2 weeks';
                
                // Update visual guide
                updateVisualGuide(isBullish, {
                    entry: entryPrice,
                    sl: slPrice,
                    tp1: tp1Price,
                    tp2: tp2Price,
                    tpMax: tpMaxPrice,
                    current: currentPrice
                }, formatPrice);
                
                // Add sample candles
                addSampleCandles(isBullish, currentPrice);
                
                // Update market status
                const statusEl = document.getElementById('marketStatus');
                statusEl.innerHTML = 
                    `<i class="fas fa-circle" style="color: ${isBullish ? '#10b981' : '#ef4444'}"></i> 
                    ${signalType} SIGNAL - ${strategyKey} CONFIRMED`;
                statusEl.style.color = isBullish ? '#10b981' : '#ef4444';
                    
            }, 2000); // Increased delay to show loading
        }

        function updateVisualGuide(isBullish, prices, formatPrice) {
            // Update labels
            document.getElementById('tpMaxLabel').textContent = `TP MAX: ${formatPrice(prices.tpMax)}`;
            document.getElementById('tp2Label').textContent = `TP2: ${formatPrice(prices.tp2)}`;
            document.getElementById('tp1Label').textContent = `TP1: ${formatPrice(prices.tp1)}`;
            document.getElementById('entryLabel').textContent = `ENTRY: ${formatPrice(prices.entry)}`;
            document.getElementById('slLabel').textContent = `SL: ${formatPrice(prices.sl)}`;
            
            // Show entry zone
            const entryZone = document.getElementById('entryZone');
            const entryArrow = document.getElementById('entryArrow');
            
            // Calculate positions
            const guideHeight = 200;
            const priceRange = Math.abs(prices.tpMax - prices.sl);
            
            // Position elements
            const getYPosition = (price) => {
                const priceDiff = isBullish ? 
                    (prices.tpMax - price) : 
                    (price - prices.sl);
                return (priceDiff / priceRange) * guideHeight;
            };
            
            // Show entry zone
            entryZone.style.display = 'block';
            entryZone.style.top = `${getYPosition(prices.entry * (isBullish ? 1.0002 : 0.9998))}px`;
            entryZone.style.height = `${Math.abs(getYPosition(prices.entry * (isBullish ? 0.9998 : 1.0002)) - getYPosition(prices.entry * (isBullish ? 1.0002 : 0.9998)))}px`;
            
            // Show entry arrow
            entryArrow.style.display = 'block';
            entryArrow.style.top = `${getYPosition(prices.current)}px`;
            entryArrow.style.left = '50%';
            entryArrow.style.transform = `translateX(-50%) rotate(${isBullish ? '0deg' : '180deg'})`;
            
            // Update instructions
            document.getElementById('entryInstructions').innerHTML = `
                <i class="fas fa-${isBullish ? 'arrow-up' : 'arrow-down'}"></i> 
                <strong>${isBullish ? 'BUY' : 'SELL'}</strong> when price enters the blue zone<br>
                <small>Wait for confirmation candle before entry • Check lower timeframe for precise entry</small>
            `;
        }

        function addSampleCandles(isBullish, currentPrice) {
            const currentData = candleSeries.data();
            const lastCandle = currentData[currentData.length - 1];
            
            const newCandles = [];
            for (let i = 1; i <= 5; i++) {
                const time = lastCandle.time + i * 900;
                const open = i === 1 ? lastCandle.close : newCandles[i-2].close;
                const change = (isBullish ? 0.0003 : -0.0003) * i;
                const close = open * (1 + change);
                const high = Math.max(open, close) + Math.random() * 0.0002;
                const low = Math.min(open, close) - Math.random() * 0.0002;
                
                newCandles.push({
                    time: time,
                    open: open,
                    high: high,
                    low: low,
                    close: close,
                });
            }
            
            // Add to chart
            candleSeries.update(newCandles[0]);
        }

        // Auto-generate on load
        window.addEventListener('load', () => {
            setTimeout(generateSignals, 1500);
        });

        // Handle window resize
        window.addEventListener('resize', () => {
            chart.applyOptions({ width: chartContainer.clientWidth });
        });
    </script>
</body>
</html>
