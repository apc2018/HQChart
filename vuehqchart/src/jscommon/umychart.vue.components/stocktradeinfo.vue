<template>
    <div class="tradeinfo" ref="tradeinfo">
        <div class="firstLine" ref="bookWrap">
            <p>
                <span>
                    委比：
                    <span :class="BookData.BookRate.Color">{{BookData.BookRate.Text}}</span>
                </span>
            </p>
            <p>
                <span>
                    委差：
                    <span :class="BookData.BookDiff.Color">{{BookData.BookDiff.Text}}</span>
                </span>
            </p>
        </div>
        <div class="sellFive">
            <div>
                <p v-for="(item,index) in SellData" :key="index">
                    <span>{{item.Title}}</span>
                </p>
            </div>
            <div>
                <p v-for="(item,index) in SellData" :key="index" :class="item.Price.Color">
                    <span>{{item.Price.Text}}</span>
                </p>
            </div>
            <div>
                <p v-for="(item,index) in SellData" :key="index">
                    <span>{{item.Vol.Text}}</span>
                </p>
            </div>
        </div>
        <div class="buyFive">
            <div>
                <p v-for="(itemBuy,ind) in BuyData" :key="ind">
                    <span>{{itemBuy.Title}}</span>
                </p>
            </div>
            <div>
                <p v-for="(itemBuy,ind) in BuyData" :key="ind" :class="itemBuy.Price.Color">
                    <span>{{itemBuy.Price.Text}}</span>
                </p>
            </div>
            <div>
                <p v-for="(itemBuy,ind) in BuyData" :key="ind">
                    <span>{{itemBuy.Vol.Text}}</span>
                </p>
            </div>
        </div>
        <div class="detailList" ref="detailList" v-show="IsShowTradeData">
            <div>
                <p v-for="(itemTrade,indexT) in TradeData" :key="indexT">
                    <span>{{itemTrade.Time.Text}}</span>
                </p>
            </div>
            <div>
                <p v-for="(itemTrade,indexT) in TradeData" :key="indexT" :class="itemTrade.Price.Color">
                    <span>{{itemTrade.Price.Text}}</span>
                </p>
            </div>
            <div>
                <p v-for="(itemTrade,indexT) in TradeData" :key="indexT">
                    <span>{{itemTrade.Vol.Text}}&nbsp;<span :class="itemTrade.BS.Color">{{itemTrade.BS.Text}}</span></span>
                </p>
            </div>
        </div>
        <div class="shorttermlist" v-show="ShortTerm.IsShow">
            <div>
                <p v-for="(item,index) in ShortTerm.Data" :key="index" :class="item.Color">{{item.Time.Text}}</p>
            </div>
            <div>
                <p v-for="(item,index) in ShortTerm.Data" :key="index" :class="item.Color">{{item.Name}}</p>
            </div>
            <div>
                <p v-for="(item,index) in ShortTerm.Data" :key="index" :class="item.Color">{{item.Type.Text}}</p>
            </div>
        </div>
    </div>
</template>
<script>
    import $ from "jquery";
    import JSCommon from "../umychart.vue/umychart.vue.js";
    import JSCommonStock from "../umychart.vue/umychart.stock.vue.js";

    //默认数据输出
    class DefaultData {
        static GetBuyData() {
            const data = [{
                    Vol: {
                        Text: ""
                    },
                    Price: {
                        Text: "",
                        Color: ""
                    },
                    Title: ""
                },
                {
                    Vol: {
                        Text: ""
                    },
                    Price: {
                        Text: "",
                        Color: ""
                    },
                    Title: ""
                },
                {
                    Vol: {
                        Text: ""
                    },
                    Price: {
                        Text: "",
                        Color: ""
                    },
                    Title: ""
                },
                {
                    Vol: {
                        Text: ""
                    },
                    Price: {
                        Text: "",
                        Color: ""
                    },
                    Title: ""
                },
                {
                    Vol: {
                        Text: ""
                    },
                    Price: {
                        Text: "",
                        Color: ""
                    },
                    Title: ""
                }
            ];

            return data;
        }

        static GetSellData() {
            return DefaultData.GetBuyData();
        }

        static GetTradeData() 
        {
            var data=[];
            for(var i=0;i<10;++i)
            {
                var item=
                { 
                    Time: {Text: ""}, 
                    Price: { Text: "", Color: ""},
                    Vol: { Text: ""}, 
                    BS:{Text:'', Color:''} 
                };

                data.push(item);
            }

            return data;
        }

        static GetBookData() {
            const data = {
                BookRate: {
                    Text: "",
                    Color: ""
                },
                BookDiff: {
                    Text: "",
                    Color: ""
                }
            };
            return data;
        }

        static GetRowStyle() {
            let data = {
                Sell: {
                    top: 0,
                    height: 0,
                    display: "none"
                },
                Buy: {
                    top: 0,
                    height: 0,
                    display: "none"
                },
                Trade: {
                    top: 0,
                    height: 0,
                    display: "none"
                }
            }; //默认都不显示

            return data;
        }

        static GetRowShortStyle() {
            let data = {
                Sell: {
                    top: 0,
                    height: 0,
                    display: "none"
                },
                Buy: {
                    top: 0,
                    height: 0,
                    display: "none"
                },
                Short: {
                    top: 0,
                    height: 0,
                    display: "none"
                }
            }; //默认都不显示

            return data;
        }

        static GetCapitalFlow()
        {
            let data={};

            return data;
        }
    }
    export default {
        JSCommonStock: JSCommonStock,

        name: "StockTradeInfo",
        props: [
            "IsShareStock" //是否共享使用一个Stock类,
        ],

        data() {
            let data = {
                Symbol: "600000.sh",
                ID: [
                    JSCommon.JSChart.CreateGuid(),
                    JSCommon.JSChart.CreateGuid(),
                    JSCommon.JSChart.CreateGuid(),
                    JSCommon.JSChart.CreateGuid()
                ], //[0]=买卖盘  [1]=分笔 [2]=委比委差 [3]=资金流
                JSStock: null,
                RowStyle: DefaultData.GetRowStyle(),

                BuyData: DefaultData.GetBuyData(), //买盘
                SellData: DefaultData.GetSellData(), //卖盘
                IsBuySellInital: false, //第1次 需要初始化

                TradeData: null, //分笔数据
                IsTradeDataInital: false, //第1次 需要初始化
                IsShowTradeData: true, //是否显示

                //短线
                ShortTerm: 
                {
                    Data: null, //短线精灵
                    IsShow: false, //是否显示
                    JSStock: null //数据请求控制器
                },

                //资金流
                CapitalFlow:
                {
                    Data: DefaultData.GetCapitalFlow(),     //数据
                    IsShow: false,  //是否显示
                },

                BookData: DefaultData.GetBookData() //委比委差数据
            };

            return data;
        },

        mounted() {
            console.log(
                `[StockTradeInfo::mounted] IsShareStock=${this.IsShareStock} ID=${
        this.ID
      }`
            );
            if (this.IsShareStock == true || this.IsShareStock == "true") {
                //外部调用SetJSStock()设置， 内部不创建
            } else {
                this.JSStock = JSCommonStock.JSStockInit();
                this.InitalStock();
                this.JSStock.RequestData();
            }

            this.OnSize();
        },

        created() {
            if (this.DefaultSymbol) this.Symbol = this.DefaultSymbol; //默认股票
        },

        methods: {
            UpdateBuySell: function (id, arySymbol, dataType, jsStock) {
                if (arySymbol.indexOf(this.Symbol) < 0) return;

                let read = jsStock.GetStockRead(this.ID[0], this.UpdateBuySell); //获取一个读取数据类,并绑定id和更新数据方法
                let aryBuy = read.Get(this.Symbol, JSCommonStock.STOCK_FIELD_NAME.BUY5);
                let arySell = read.Get(this.Symbol, JSCommonStock.STOCK_FIELD_NAME.SELL5); //买盘数据需要倒序排列
                let yClose = read.Get(this.Symbol, JSCommonStock.STOCK_FIELD_NAME.YCLOSE);

                let SellTitle = ["卖一", "卖二", "卖三", "卖四", "卖五"];
                let BuyTitle = ["买一", "买二", "买三", "买四", "买五"];

                if (!this.IsPlusNumber(yClose)) return;

                if (
                    this.IsBuySellInital == true &&
                    (id != this.ID[0] ||
                        dataType != JSCommonStock.RECV_DATA_TYPE.BUY_SELL_DATA)
                )
                    return;

                console.log(
                    "[StockTradeInfo::UpdateBuySell]",
                    this.Symbol,
                    aryBuy,
                    arySell,
                    yClose
                );
                let buyData = DefaultData.GetBuyData();
                for (var i in aryBuy) {
                    var item = aryBuy[i];
                    var buyItem = buyData[i];
                    buyItem.Vol.Text = item.Vol.toString();
                    buyItem.Title = BuyTitle[i];
                    buyItem.Price.Text = JSCommon.IFrameSplitOperator.FormatValueString(
                        item.Price,
                        2
                    );
                    buyItem.Price.Color = JSCommon.IFrameSplitOperator.FormatValueColor(
                        item.Price,
                        yClose
                    );
                }

                let sellData = DefaultData.GetSellData();
                for (var i in arySell) {
                    var item = arySell[i];
                    var selItem = sellData[i];
                    selItem.Vol.Text = item.Vol.toString();
                    selItem.Title = SellTitle[i];
                    selItem.Price.Text = JSCommon.IFrameSplitOperator.FormatValueString(
                        item.Price,
                        2
                    );
                    selItem.Price.Color = JSCommon.IFrameSplitOperator.FormatValueColor(
                        item.Price,
                        yClose
                    );
                }

                this.IsBuySellInital = true; //已经初始化了
                this.SellData = sellData.reverse();
                this.BuyData = buyData;
            },
            //设置外部共享的股票数据类
            SetJSStock: function (jsStock) {
                this.JSStock = jsStock;
            },

            //窗口变化UI自适应调整
            OnSize() {},

            RequestData: function () {
                this.JSStock.RequestData();
            },

            UpdateBookRate: function (id, arySymbol, dataType, jsStock) {
                if (arySymbol.indexOf(this.Symbol) < 0) return;

                let read = jsStock.GetStockRead(this.ID[2], this.UpdateBookRate); //获取一个读取数据类,并绑定id和更新数据方法
                var bookRate = {
                    Value: read.Get(this.Symbol, JSCommonStock.STOCK_FIELD_NAME.BOOK_RATE)
                }; //委比
                var bookDiffer = {
                    Value: read.Get(this.Symbol, JSCommonStock.STOCK_FIELD_NAME.BOOK_DIFFER)
                }; //委差

                if (
                    id != this.ID[2] ||
                    dataType != JSCommonStock.RECV_DATA_TYPE.DERIVATIVE_DATA
                )
                    return;
                let bookData = DefaultData.GetBookData();
                bookData.BookRate.Text =
                    JSCommon.IFrameSplitOperator.FormatValueString(bookRate.Value, 2) + "%";
                bookData.BookRate.Color = JSCommon.IFrameSplitOperator.FormatValueColor(
                    bookRate.Value,
                    0
                );
                bookData.BookDiff.Text = bookDiffer.Value;
                bookData.BookDiff.Color = JSCommon.IFrameSplitOperator.FormatValueColor(
                    bookDiffer.Value,
                    0
                );
                this.BookData = bookData;
                // console.log('[UpdateBookRate BookData]',this.BookData);
            },

            UpdateTrade: function (id, arySymbol, dataType, jsStock) {
                if (arySymbol.indexOf(this.Symbol) < 0) return;

                let read = jsStock.GetStockRead(this.ID[1], this.UpdateTrade); //获取一个读取数据类,并绑定id和更新数据方法
                let aryDeal = read.Get(this.Symbol, JSCommonStock.STOCK_FIELD_NAME.DEAL);
                let yClose = read.Get(this.Symbol, JSCommonStock.STOCK_FIELD_NAME.YCLOSE);

                console.log(
                    "[StockTradeInfo::UpdateTrade]",
                    this.Symbol,
                    aryDeal,
                    yClose
                );

                if (!this.IsPlusNumber(yClose) || !aryDeal) return;
                if ( this.IsTradeDataInital == true && (id != this.ID[1] || dataType != JSCommonStock.RECV_DATA_TYPE.DEAL_DATA)) return;

                let data = DefaultData.GetTradeData();
                for (let i = 0; i < aryDeal.length && i < data.length; i++) 
                {
                    var item = aryDeal[i];
                    var tradeItem = data[i];
                    var newTime = parseInt(item.Time / 100); //去掉秒

                    tradeItem.Time.Text = JSCommon.IFrameSplitOperator.FormatTimeString(newTime);
                    tradeItem.Price.Text = JSCommon.IFrameSplitOperator.FormatValueString(item.Price,2);
                    tradeItem.Price.Color = JSCommon.IFrameSplitOperator.FormatValueColor(item.Price,yClose);
                    tradeItem.Vol.Text = parseInt(item.Vol / 100).toString(); //单位是股 需要/100
                   
                    if (item.BS=='B') tradeItem.BS={Color:'PriceUp', Text:' B'};
                    else if (item.BS=='S') tradeItem.BS={Color:'PriceDown', Text:' S'};
                    else tradeItem.BS={Color:'Transparent', Text:' B'}; //使用透明色
                }

                this.IsTradeDataInital = true; //已经初始化了
                this.TradeData = data;
            },

            //资金流
            InitalCapitalFlow:function()
            {
                //订阅字段
                var field=
                [
                    JSCommonStock.STOCK_FIELD_NAME.CAPITAL_FLOW_DAY,
                    JSCommonStock.STOCK_FIELD_NAME.CAPITAL_FLOW_DAY3,
                    JSCommonStock.STOCK_FIELD_NAME.CAPITAL_FLOW_DAY5,
                    JSCommonStock.STOCK_FIELD_NAME.CAPITAL_FLOW_DAY10,
                    //JSCommonStock.STOCK_FIELD_NAME.DDE,
                    //JSCommonStock.STOCK_FIELD_NAME.DDE3,
                    //JSCommonStock.STOCK_FIELD_NAME.DDE5,
                    //JSCommonStock.STOCK_FIELD_NAME.DDE10,
                ];
                var read = this.JSStock.GetStockRead(this.ID[3], this.UpdateCapitalFlow); //订阅资金流
                read.SetQueryField(this.Symbol, field);
            },

            UpdateCapitalFlow:function(id, arySymbol, dataType, jsStock)
            {
                if (!this.CapitalFlow.IsShow) return;    //不显示直接返回 不订阅了
                if (arySymbol.indexOf(this.Symbol) < 0) return;

                let read = jsStock.GetStockRead(this.ID[3], this.UpdateCapitalFlow); //获取一个读取数据类,并绑定id和更新数据方法
                var flowDay = read.Get(this.Symbol, JSCommonStock.STOCK_FIELD_NAME.CAPITAL_FLOW_DAY);
                var flowDay3 = read.Get(this.Symbol, JSCommonStock.STOCK_FIELD_NAME.CAPITAL_FLOW_DAY3);
                var flowDay5 = read.Get(this.Symbol, JSCommonStock.STOCK_FIELD_NAME.CAPITAL_FLOW_DAY5);
                var flowDay10 = read.Get(this.Symbol, JSCommonStock.STOCK_FIELD_NAME.CAPITAL_FLOW_DAY10);

                console.log("[StockTradeInfo::UpdateCapitalFlow]", this.Symbol, flowDay, flowDay3,flowDay5,flowDay10);
            },

            //切换股票代码
            SetSymbol: function (symbol) {
                if (this.Symbol == symbol) return;

                console.log(`[StockTradeInfo::SetSymbol] symbol=${this.Symbol}`);

                this.Symbol = symbol;
                this.IsBuySellInital = false;
                this.IsTradeDataInital = false;
                this.InitalStock(); //订阅数据
                if (!this.IsShareStock) this.JSStock.RequestData();
            },

            //初始化
            InitalStock: function () {
                if (this.IsSHSZIndex()) return; //指数没有买卖盘

                // 分成3个ID 分开取
                let read = this.JSStock.GetStockRead(this.ID[0], this.UpdateBuySell); //订阅买卖盘数据
                read.SetQueryField(this.Symbol, [
                    JSCommonStock.STOCK_FIELD_NAME.BUY5,
                    JSCommonStock.STOCK_FIELD_NAME.SELL5,
                    JSCommonStock.STOCK_FIELD_NAME.YCLOSE
                ]);

                read = this.JSStock.GetStockRead(this.ID[1], this.UpdateTrade); //订阅成交明细
                read.SetQueryField(this.Symbol, [
                    JSCommonStock.STOCK_FIELD_NAME.DEAL,
                    JSCommonStock.STOCK_FIELD_NAME.YCLOSE
                ]);

                read = this.JSStock.GetStockRead(this.ID[2], this.UpdateBookRate); //订阅委比委差
                read.SetQueryField(this.Symbol, [
                    JSCommonStock.STOCK_FIELD_NAME.BOOK_RATE,
                    JSCommonStock.STOCK_FIELD_NAME.BOOK_DIFFER
                ]);
            },

            IsSHSZIndex: function () {
                return JSCommon.MARKET_SUFFIX_NAME.IsSHSZIndex(this.Symbol);
            },

            //改变显示数据
            ChangeShowData: function (name, value) {
                console.log("[StockTradeInfo::ChangeShowData]", name, value);
                switch (name) 
                {
                    case "分笔":
                        this.IsShowTradeData = true;
                        this.ShowShortTerm(false);
                        this.ShowCapitalFlow(false);
                        this.OnSize();
                        break;
                    case "异动":
                        this.IsShowTradeData = false;
                        this.ShowCapitalFlow(false);
                        this.ShowShortTerm(true);
                        break;
                    case '资金':
                        this.IsShowTradeData = false;
                        this.ShowShortTerm(false);
                        this.ShowCapitalFlow(true);
                        break;
                }
            },

            //移动显示
            ShowShortTerm: function (isShow) 
            {
                if (isShow) 
                {
                    if (!this.ShortTerm.JSStock) this.ShortTerm.JSStock = JSCommonStock.JSStock.GetShortTerm();
                    var shortTerm = this.ShortTerm.JSStock;
                    shortTerm.UpdateUICallback = this.UpdateShortTerm;
                    shortTerm.IsAutoUpdate = true;
                    shortTerm.RequestData();
                    this.ShortTerm.IsShow = true;
                } else 
                {
                    this.ShortTerm.IsShow = false;
                    if (!this.ShortTerm.JSStock) return;
                    this.ShortTerm.JSStock.IsAutoUpdate = false;
                    this.ShortTerm.JSStock.Stop();
                }
            },

            //资金流显示
            ShowCapitalFlow:function(isShow)
            {
                if (isShow) 
                {
                    this.CapitalFlow.IsShow=true;
                    this.InitalCapitalFlow();
                    this.RequestData();
                } else 
                {
                    this.CapitalFlow.IsShow=false;
                }
            },

            UpdateShortTerm: function (data) {
                console.log("[StockTradeInfo::UpdateShortTerm] recv data", data);
                var showData = [];
                for (var i in data.Data) {
                    var item = data.Data[i];
                    var showItem = {
                        Name: item.Name,
                        Symbol: item.Symbol,
                        Type: {
                            Text: item.TypeInfo,
                            Value: item.Type
                        },
                        Time: {
                            Value: item.Time
                        },
                        Color: "PriceNull"
                    };
                    showItem.Time.Text = JSCommon.IFrameSplitOperator.FormatTimeString(
                        parseInt(showItem.Time.Value / 100)
                    );
                    showItem.Color =
                        showItem.Type.Value % 10 === 1 ? "PriceUp" : "PriceDown";
                    showData.push(showItem);
                }
                this.ShortTerm.Data = showData;
                this.OnSize();
            },

            IsNumber: JSCommon.IFrameSplitOperator.IsNumber, //是否是数值型
            IsPlusNumber: JSCommon.IFrameSplitOperator.IsPlusNumber //是否是>0的数值型
        }
    };
</script>

<style lang="scss" scoped>
    $border: 1px solid #e1ecf2;

    html,
    body,
    div,
    p,
    span,
    li {
        font: 14px / normal "Microsoft Yahei";
    }

    * {
        padding: 0;
        margin: 0;
    }

    ol,
    ul {
        list-style: none;
    }

    .PriceUp,
    .PriceUp>span {
        color: #ee1515 !important;
    }

    .PriceDown,
    .PriceDown>span {
        color: #199e00 !important;
    }

    .PriceNull {
        color: inherit;
    }

    .Transparent,
    {
        color:rgba(255,0,255,0);
    }

    .tradeinfo {
        border: $border;
        width: 100%;
        height: 100%;

        .firstLine {
            width: 100%;
            border-bottom: $border;
            padding-left: 10px;
            display: flex;
            flex-direction: row;
            flex-wrap: wrap;
            box-sizing: border-box;

            >p {
                flex-grow: 1;
                line-height: 67px;
            }
        }

        .buyFive,
        .sellFive,
        .detailList,
        .shorttermlist {
            border-bottom: $border;
            padding: 0 10px;
            display: flex;
            flex-direction: row;
            flex-wrap: wrap;
            overflow: hidden;
            width: 100%;
            box-sizing: border-box;

            >div {
                flex-grow: 1;
                height: 100%;

                p {
                    line-height: 24px;
                }
            }

            >div:nth-of-type(3),
            >div:nth-of-type(2) {
                p {
                    width: 100%;
                    text-align: right;
                }
            }
        }

        .detailList,
        .shorttermlist {

            border-bottom: none;
        }
    }
</style>