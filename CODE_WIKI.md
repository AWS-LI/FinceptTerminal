# Fincept Terminal v4.0.2 - Code Wiki Documentation

## 1. 项目概述

### 1.1 项目简介
Fincept Terminal是一个功能强大的纯原生C++20桌面金融智能平台，使用Qt6进行界面和渲染，通过嵌入的Python脚本执行高级分析。该平台提供彭博终端级别的性能，是一个集数据收集、分析、交易和AI智能于一体的完整金融工具。

### 1.2 主要特性
- **CFA级别分析**：折现现金流模型、投资组合优化、风险评估指标
- **AI智能代理**：37个各类代理（交易员、投资者、经济学家、地缘政治分析师）
- **100+ 数据连接器**：包括DBnomics、Polygon、Kraken、Yahoo Finance、FRED、IMF等
- **实时交易**：加密货币（Kraken/Hyperliquid）、股票、算法交易、模拟交易引擎
- **多经纪商集成**：支持16家经纪商（Zerodha、AngelOne、Upstox、Fyers、Dhan、Groww、Kotak、IIFL、5paisa、AliceBlue、Shoonya、Motilal、IBKR、Alpaca、Tradier、Saxo）
- **量化分析套件**：18个量化分析模块
- **全球智能**：海事跟踪、地缘政治分析、关系图谱、卫星数据
- **可视化工作流**：用于自动化流程的节点编辑器
- **AI量化实验室**：机器学习模型、因子发现、高频交易、强化学习交易

### 1.3 技术栈
| 组件 | 技术 | 用途 |
| ---- | ---- | ---- |
| 编程语言 | C++20 | 核心应用 |
| UI框架 | Qt6 Widgets | 原生保留模式图形界面 |
| 图表库 | Qt6 Charts | 金融图表与绘图 |
| 网络库 | Qt6 Network | HTTP API调用、TLS |
| WebSocket | Qt6 WebSockets | 实时数据流 |
| 数据库 | Qt6 Sqlite | 本地存储与缓存 |
| 数据分析 | Python 3.11+ | 嵌入式运行时执行脚本 |
| 构建系统 | CMake 3.27+ | 项目构建 |
| 文档格式 | Markdown | 项目文档 |

---

## 2. 系统架构

### 2.1 整体架构图
```
┌─────────────────────────────────────────────────────────────────────────┐
│                           用户界面层                                     │
│  ┌──────────────────────────────────────────────────────────────────┐  │
│  │  Qt6 Widgets + Qt6 Charts                                         │  │
│  │  - Obsidian 设计系统 (彭博风格终端界面)                           │  │
│  │  - 实时数据可视化                                                 │  │
│  │  - 原生平台渲染                                                   │  │
│  └──────────────────────────────────────────────────────────────────┘  │
├─────────────────────────────────────────────────────────────────────────┤
│                          应用服务层                                      │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐│
│  │   屏幕 UI    │  │   数据服务   │  │  交易引擎    │  │ MCP集成层    ││
│  │  (40+ 屏幕)  │  │  (市场/新闻) │  │  (16家经纪商) │  │ (AI工具系统) ││
│  └──────────────┘  └──────────────┘  └──────────────┘  └──────────────┘│
├─────────────────────────────────────────────────────────────────────────┤
│                        基础设施层                                        │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐│
│  │ HTTP客户端   │  │   SQLite数据库│  │WebSocket客户端│  │Python运行器  ││
│  │  (Qt Network)│  │ (Qt Sql)     │  │  (Qt WS)     │  │ (100+脚本)   ││
│  └──────────────┘  └──────────────┘  └──────────────┘  └──────────────┘│
├─────────────────────────────────────────────────────────────────────────┤
│                         平台层                                           │
│  ┌──────────────────────────────────────────────────────────────────┐  │
│  │  Qt6 平台抽象层                                                 │  │
│  │  Windows (MSVC) / macOS (Clang) / Linux (GCC)                  │  │
│  └──────────────────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────────────────┘
```

### 2.2 目录结构
```
/workspace/
├── .github/
│   ├── ISSUE_TEMPLATE/
│   ├── PULL_REQUEST_TEMPLATE/
│   ├── workflows/
│   ├── CONTRIBUTING.md
│   └── FUNDING.yml
├── docs/
│   ├── translations/
│   ├── ARCHITECTURE.md
│   ├── CODE_OF_CONDUCT.md
│   ├── COMMERCIAL_LICENSE.md
│   ├── CONTRIBUTING.md
│   ├── CPP_CONTRIBUTOR_GUIDE.md
│   ├── GETTING_STARTED.md
│   └── PYTHON_CONTRIBUTOR_GUIDE.md
├── fincept-qt/
│   ├── .claude/
│   ├── cmake/
│   ├── docs/
│   │   ├── agents/
│   │   ├── datahub-phases/
│   │   └── (数据连接阶段文档)
│   ├── resources/
│   ├── scripts/
│   │   ├── Analytics/
│   │   ├── agents/
│   │   ├── algo_trading/
│   │   ├── exchange/
│   │   ├── mcp/
│   │   ├── strategies/
│   │   └── (各种数据获取脚本)
│   ├── src/
│   │   ├── app/
│   │   ├── core/
│   │   ├── ui/
│   │   ├── network/
│   │   ├── storage/
│   │   ├── auth/
│   │   ├── python/
│   │   ├── trading/
│   │   ├── services/
│   │   ├── screens/
│   │   ├── mcp/
│   │   ├── datahub/
│   │   └── ai_chat/
│   ├── CMakeLists.txt
│   └── CMakePresets.json
├── LICENSE
└── README.md
```

### 2.3 设计模式
- **屏幕/服务分离**：屏幕负责UI，服务负责数据获取和处理
- **单例模式**：核心服务采用单例实现
- **事件总线**：解耦组件间通信
- **结果类型**：使用`Result<T>`代替异常进行错误处理
- **依赖注入**：通过构造函数注入依赖
- **工厂模式**：UI组件、图表等创建

---

## 3. 核心模块详解

### 3.1 应用层 (app/)

#### 3.1.1 主入口 - main.cpp
**位置**: [fincept-qt/src/app/main.cpp](file:///workspace/fincept-qt/src/app/main.cpp)  
**职责**: 应用程序启动入口，负责初始化所有核心组件

**主要功能**:
- 解析命令行参数（如 `--profile`）
- 配置SingleApplication（单实例+多窗口支持）
- 注册DataHub元类型
- 初始化应用目录
- 数据库迁移
- 日志系统
- 主题应用
- Python环境检查
- 启动主窗口或设置向导

**关键代码片段**:
```cpp
int main(int argc, char* argv[]) {
    // 解析 --profile 参数
    // SingleApplication初始化
    // DataHub注册
    // 数据库打开与迁移
    // 主题应用
    // Python环境检查
    // 主窗口创建
}
```

#### 3.1.2 主窗口 - MainWindow
**位置**: [fincept-qt/src/app/MainWindow.h](file:///workspace/fincept-qt/src/app/MainWindow.h)  
**职责**: 应用程序主窗口，管理整个界面布局和导航

**主要属性**:
- `stack_`: 主屏幕堆栈
- `auth_stack_`: 认证屏幕堆栈
- `dock_manager_`: Qt Advanced Docking System (ADS) 管理器
- `dock_router_`: 停靠屏幕路由
- `chat_bubble_`: AI聊天小部件
- `lock_screen_`: 锁定/PIN屏幕
- `window_id_`: 窗口ID（0为主窗口，1+为副窗口）

**主要方法**:
- `MainWindow(int window_id = 0)`: 构造函数，创建新窗口
- `closeEvent()`: 窗口关闭事件处理
- `setup_auth_screens()`: 设置认证屏幕
- `setup_docking_mode()`: 设置停靠模式
- `toggle_chat_mode()`: 切换聊天模式
- `show_lock_screen()`: 显示锁定屏幕

---

### 3.2 核心层 (core/)

#### 3.2.1 应用配置 - AppConfig
**位置**: [fincept-qt/src/core/config/AppConfig.h](file:///workspace/fincept-qt/src/core/config/AppConfig.h)  
**职责**: 管理应用程序持久化配置

**主要方法**:
- `instance()`: 获取单例
- `get(key, default)`: 获取配置项
- `set(key, value)`: 设置配置项
- `remove(key)`: 删除配置项
- `api_base_url()`: 获取API基础URL
- `dark_mode()`: 获取是否深色模式
- `refresh_interval_ms()`: 获取刷新间隔

**实现**: 使用QSettings作为底层存储

#### 3.2.2 事件总线 - EventBus
**职责**: 实现发布-订阅模式，解耦组件间通信

**主要方法**:
- `instance()`: 获取单例
- `subscribe(event, callback)`: 订阅事件
- `publish(event, data)`: 发布事件
- `unsubscribe(subscription_id)`: 取消订阅

#### 3.2.3 日志系统 - Logger
**职责**: 结构化日志记录

**日志级别**: Trace, Debug, Info, Warn, Error, Fatal

**主要方法**:
- `instance()`: 获取单例
- `trace(tag, msg)`: 跟踪级别日志
- `debug(tag, msg)`: 调试级别日志
- `info(tag, msg)`: 信息级别日志
- `warn(tag, msg)`: 警告级别日志
- `error(tag, msg)`: 错误级别日志
- `fatal(tag, msg)`: 致命级别日志
- `set_level(level)`: 设置全局日志级别
- `set_tag_level(tag, level)`: 设置特定标签日志级别
- `set_json_mode(bool)`: 设置JSON输出模式

#### 3.2.4 结果类型 - Result
**职责**: 类型安全的错误处理，避免异常

**结构**:
```cpp
template <typename T>
class Result {
  public:
    bool is_ok() const;
    bool is_err() const;
    T& value();
    const std::string& error() const;
    
    static Result ok(T val);
    static Result err(std::string err);
};
```

---

### 3.3 用户界面层 (ui/)

#### 3.3.1 主题系统
**主题**: Obsidian（彭博风格深色主题）

**相关类**:
- `Theme`: 主题颜色和字体常量
- `ThemeManager`: 主题管理
- `StyleSheets`: Qt样式表集合

#### 3.3.2 导航组件
- `NavigationBar`: 左侧导航栏
- `DockToolBar`: 停靠工具栏
- `DockStatusBar`: 停靠状态栏
- `TabBar`: 标签栏
- `FKeyBar`: 功能键快捷栏

#### 3.3.3 通用小部件
- `Card`: 卡片容器
- `SearchBar`: 搜索栏
- `StatusBadge`: 状态标签
- `WorldMapWidget`: 世界地图小部件
- `LoadingOverlay`: 加载覆盖层
- `NotifToast`: 通知提示
- `NotifBell`: 通知铃铛
- `GeometricBackground`: 几何背景

#### 3.3.4 数据可视化
- `DataTable`: 可重用数据表格
- `ChartFactory`: Qt6图表工厂
- `MarkdownRenderer`: Markdown渲染器

---

### 3.4 网络层 (network/)

#### 3.4.1 HTTP客户端 - HttpClient
**职责**: 发送HTTP请求和处理响应

**主要方法**:
- `instance()`: 获取单例
- `set_base_url(url)`: 设置基础URL
- `get(path, callback)`: GET请求
- `post(path, data, callback)`: POST请求
- `put(path, data, callback)`: PUT请求
- `delete_(path, callback)`: DELETE请求

#### 3.4.2 WebSocket客户端 - WebSocketClient
**职责**: 管理WebSocket连接，处理实时数据流

**主要方法**:
- `instance()`: 获取单例
- `connect(url)`: 连接WebSocket
- `disconnect()`: 断开连接
- `send(message)`: 发送消息
- `is_connected()`: 检查连接状态
- `on_message(callback)`: 注册消息回调

---

### 3.5 存储层 (storage/)

#### 3.5.1 数据库管理
**主数据库**: fincept.db (SQLite)  
**缓存数据库**: cache.db (SQLite)

**表名前缀**:
- `settings_*`: 应用设置
- `watchlist_*`: 观察列表
- `chat_*`: 聊天记录
- `portfolio_*`: 投资组合
- `mcp_*`: MCP配置
- `news_*`: 新闻数据

#### 3.5.2 数据迁移系统
**版本化迁移**: 每个迁移都有版本号（v001, v002, ...）

**迁移列表**:
- v001: 初始架构
- v002: LLM聊天表
- v003: 数据MCP代理表
- v004: 清理
- v005: 仪表板布局
- v006: 多投资组合
- v007: 新闻基准
- v008: 工作流
- v009: 工具master合约
- v010: LLM配置
- v011: 自定义指数
- v012: 数据归一化
- v013: 新闻文章
- v014: LLM工具切换
- v015: 屏幕UI状态
- v016: 经纪商账户
- v017: 性能索引
- v018: 小部件配置

**相关类**:
- `Database`: 主数据库管理
- `CacheDatabase`: 缓存数据库管理
- `MigrationRunner`: 迁移执行器

#### 3.5.3 安全存储 - SecureStorage
**职责**: 加密存储敏感数据（API密钥、密码等）

**平台支持**:
- Windows: DPAPI
- macOS: Keychain
- Linux: libsecret / GNOME Keyring

#### 3.5.4 数据仓库模式
**仓库类列表**:
| 仓库名 | 用途 |
| ------ | ---- |
| `SettingsRepository` | 应用设置 |
| `WatchlistRepository` | 观察列表 |
| `ChatRepository` | 聊天记录 |
| `PortfolioRepository` | 投资组合 |
| `PortfolioHoldingsRepository` | 投资组合持仓 |
| `PaperTradingRepository` | 模拟交易 |
| `NewsMonitorRepository` | 新闻监控 |
| `NewsArticleRepository` | 新闻文章 |
| `LlmConfigRepository` | LLM配置 |
| `LlmProfileRepository` | LLM配置文件 |
| `DataSourceRepository` | 数据源 |
| `McpServerRepository` | MCP服务器 |
| `AgentConfigRepository` | 代理配置 |
| `ContextRecordingRepository` | 上下文记录 |
| `WorkflowRepository` | 工作流 |
| `CustomIndexRepository` | 自定义指数 |
| `DataMappingRepository` | 数据映射 |
| `AccountRepository` | 账户 |

---

### 3.6 认证层 (auth/)

#### 3.6.1 认证管理器 - AuthManager
**职责**: 管理用户认证、会话、令牌

**主要功能**:
- 登录/登出
- 会话验证
- 令牌刷新
- 访客模式

#### 3.6.2 会话守护 - SessionGuard
**职责**: 监听401响应，自动登出

#### 3.6.3 PIN管理器 - PinManager
**职责**: 管理PIN锁定功能

#### 3.6.4 非活动守护 - InactivityGuard
**职责**: 空闲超时后自动锁定

---

### 3.7 Python集成层 (python/)

#### 3.7.1 Python运行器 - PythonRunner
**位置**: [fincept-qt/src/python/PythonRunner.h](file:///workspace/fincept-qt/src/python/PythonRunner.h)  
**职责**: 执行Python脚本，管理Python子进程

**主要方法**:
- `instance()`: 获取单例
- `run(script, args, callback, on_line)`: 异步运行脚本
- `run_code(code, callback)`: 运行任意Python代码
- `python_path()`: 获取Python可执行文件路径
- `scripts_dir()`: 获取脚本目录
- `is_available()`: 检查Python是否可用
- `set_max_concurrent(n)`: 设置最大并发数（默认3）

**结果结构**:
```cpp
struct PythonResult {
    bool success = false;
    QString output;
    QString error;
    int exit_code = -1;
};
```

#### 3.7.2 Python环境管理器 - PythonSetupManager
**职责**: 管理Python虚拟环境和依赖包

**主要方法**:
- `instance()`: 获取单例
- `check_status()`: 检查环境状态
- `run_setup()`: 运行设置
- `needs_setup`: 是否需要设置
- `needs_package_sync`: 是否需要同步包

#### 3.7.3 脚本目录结构
**脚本位置**: fincept-qt/scripts/

**子目录**:
| 目录 | 用途 |
| ---- | ---- |
| Analytics/ | 金融分析脚本 |
| agents/ | AI代理脚本 |
| algo_trading/ | 算法交易脚本 |
| exchange/ | 交易所API客户端 |
| mcp/ | MCP工具脚本 |
| strategies/ | 交易策略脚本 |
| ai_quant_lab/ | AI量化实验室脚本 |

---

### 3.8 交易层 (trading/)

#### 3.8.1 经纪商接口 - IBroker
**位置**: [fincept-qt/src/trading/BrokerInterface.h](file:///workspace/fincept-qt/src/trading/BrokerInterface.h)  
**职责**: 所有经纪商集成的抽象基类

**主要方法**:
- `id()`: 获取经纪商ID
- `name()`: 获取经纪商名称
- `profile()`: 获取经纪商配置文件
- `exchange_token()`: 交换令牌
- `place_order()`: 下单
- `modify_order()`: 修改订单
- `cancel_order()`: 取消订单
- `get_orders()`: 获取订单
- `get_positions()`: 获取持仓
- `get_holdings()`: 获取持有资产
- `get_funds()`: 获取资金
- `get_quotes()`: 获取报价
- `get_history()`: 获取历史数据

**支持的经纪商**:
1. **Zerodha** (印度)
2. **AngelOne** (印度)
3. **Upstox** (印度)
4. **Fyers** (印度)
5. **Dhan** (印度)
6. **Groww** (印度)
7. **Kotak** (印度)
8. **IIFL** (印度)
9. **5paisa** (印度)
10. **AliceBlue** (印度)
11. **Shoonya** (印度)
12. **Motilal** (印度)
13. **IBKR** (美国/全球)
14. **Alpaca** (美国)
15. **Tradier** (美国)
16. **SaxoBank** (欧洲/全球)

#### 3.8.2 经纪商注册表 - BrokerRegistry
**职责**: 注册和查找经纪商实现

#### 3.8.3 统一交易门面 - UnifiedTrading
**职责**: 为所有经纪商提供统一的交易接口

#### 3.8.4 模拟交易引擎 - PaperTrading
**职责**: 无风险模拟交易

**功能**:
- 订单匹配
- 持仓跟踪
- P&L计算
- 佣金模拟

#### 3.8.5 交易所服务 - ExchangeService
**职责**: 交易所连接管理（Kraken、Hyperliquid等）

---

### 3.9 服务层 (services/)

#### 3.9.1 市场数据服务 - MarketDataService
**职责**: 获取和缓存市场数据

**支持的数据源**:
- Yahoo Finance
- Polygon
- DBnomics
- FRED
- IMF
- World Bank
- AkShare (中国市场)
- Alpha Vantage
- 等等...

#### 3.9.2 新闻服务 - NewsService
**职责**: 聚合和管理金融新闻

**功能**:
- 多源新闻聚合
- 新闻聚类
- 新闻监控
- 新闻情感分析
- 新闻相关性分析

#### 3.9.3 股票研究服务 - EquityResearchService
**职责**: 提供深度股票研究功能

**功能**:
- 公司概况
- 财务报表
- 技术分析
- 基本面分析
- 同行比较
- 市场情绪

#### 3.9.4 投资组合服务 - PortfolioService
**职责**: 管理投资组合

**功能**:
- 多投资组合支持
- 持仓管理
- 性能跟踪
- 风险分析
- 优化建议

#### 3.9.5 工作流服务 - WorkflowService
**职责**: 管理可视化工作流

**节点类型**:
- 触发器节点
- 控制流节点
- 市场数据节点
- 交易节点
- 分析节点
- 通知节点
- 代理节点
- 文件节点
- 数据格式节点
- 集成节点

#### 3.9.6 代理服务 - AgentService
**职责**: 管理AI代理

**代理类型**:
- 交易员代理
- 投资者代理
- 经济学家代理
- 地缘政治分析师代理

---

### 3.10 屏幕层 (screens/)

#### 3.10.1 认证屏幕
- `LoginScreen`: 登录屏幕
- `RegisterScreen`: 注册屏幕
- `ForgotPasswordScreen`: 忘记密码屏幕
- `PricingScreen`: 价格计划屏幕
- `LockScreen`: PIN锁定屏幕

#### 3.10.2 核心功能屏幕
- `DashboardScreen`: 仪表板（可配置小部件）
- `MarketsScreen`: 市场屏幕
- `EquityResearchScreen`: 股票研究屏幕
- `PortfolioScreen`: 投资组合屏幕
- `NewsScreen`: 新闻屏幕
- `CryptoTradingScreen`: 加密货币交易屏幕
- `EquityTradingScreen`: 股票交易屏幕
- `WatchlistScreen`: 观察列表屏幕

#### 3.10.3 分析屏幕
- `QuantLibScreen`: QuantLib分析屏幕
- `MAnalyticsScreen`: M&A分析屏幕
- `SurfaceAnalyticsScreen`: 曲面分析屏幕
- `BacktestingScreen`: 回测屏幕
- `AlgoTradingScreen`: 算法交易屏幕
- `AIQuantLabScreen`: AI量化实验室屏幕
- `AlphaArenaScreen`: Alpha竞技场屏幕

#### 3.10.4 数据和智能屏幕
- `DataSourcesScreen`: 数据源屏幕
- `DBnomicsScreen`: DBnomics数据屏幕
- `GovDataScreen`: 政府数据屏幕
- `EconomicsScreen`: 经济学屏幕
- `GeopoliticsScreen`: 地缘政治屏幕
- `MaritimeScreen`: 海事智能屏幕
- `RelationshipMapScreen`: 关系图谱屏幕
- `PolymarketScreen`: Polymarket屏幕

#### 3.10.5 AI和自动化屏幕
- `AiChatScreen`: AI聊天屏幕
- `ChatModeScreen`: 聊天模式屏幕
- `AgentConfigScreen`: 代理配置屏幕
- `NodeEditorScreen`: 节点编辑器屏幕
- `McpServersScreen`: MCP服务器屏幕

#### 3.10.6 实用工具屏幕
- `FileManagerScreen`: 文件管理器屏幕
- `CodeEditorScreen`: 代码编辑器屏幕
- `ExcelScreen`: Excel表格屏幕
- `ReportBuilderScreen`: 报告构建器屏幕
- `NotesScreen`: 笔记屏幕
- `SettingsScreen`: 设置屏幕
- `ProfileScreen`: 个人资料屏幕

#### 3.10.7 信息屏幕
- `AboutScreen`: 关于屏幕
- `SupportScreen`: 支持屏幕
- `ContactScreen`: 联系屏幕
- `TermsScreen`: 条款屏幕
- `PrivacyScreen`: 隐私屏幕
- `TrademarksScreen`: 商标屏幕
- `HelpScreen`: 帮助屏幕

---

### 3.11 MCP层 (mcp/)

#### 3.11.1 MCP管理器 - McpManager
**职责**: 管理MCP（Model Context Protocol）服务器

#### 3.11.2 MCP服务 - McpService
**职责**: 提供MCP工具集成

#### 3.11.3 MCP工具列表
| 工具 | 用途 |
| ---- | ---- |
| `NavigationTools` | 导航控制 |
| `MarketsTools` | 市场操作 |
| `WatchlistTools` | 观察列表操作 |
| `NewsTools` | 新闻操作 |
| `NotesTools` | 笔记操作 |
| `FileManagerTools` | 文件管理 |
| `AiChatTools` | AI聊天 |
| `PortfolioTools` | 投资组合操作 |
| `CryptoTradingTools` | 加密货币交易 |
| `PaperTradingTools` | 模拟交易 |
| `EdgarTools` | EDGAR数据 |
| `MAnalyticsTools` | M&A分析 |
| `AltInvestmentsTools` | 另类投资 |
| `DataSourcesTools` | 数据源操作 |
| `ForumTools` | 论坛操作 |
| `ProfileTools` | 个人资料操作 |
| `SettingsTools` | 设置操作 |
| `PythonTools` | Python执行 |
| `SystemTools` | 系统操作 |
| `DataHubTools` | DataHub操作 |

---

### 3.12 DataHub层 (datahub/)

#### 3.12.1 DataHub核心
**职责**: 统一的数据分发系统

**架构阶段**:
- **Phase 0**: 基础架构和元类型
- **Phase 1**: 工具master合约和DataHub架构
- **Phase 2**: 市场数据生产者
- **Phase 3**: 市场数据完整迁移
- **Phase 4**: WebSocket生产者
- **Phase 5**: 新闻生产者
- **Phase 6**: 经济学/DBnomics生产者
- **Phase 7**: 经纪商账户流
- **Phase 8**: 地缘政治/海事/关系图谱
- **Phase 9**: AI MCP代理
- **Phase 10**: 执行清理

#### 3.12.2 生产者接口 - Producer
**职责**: 产生数据到DataHub

#### 3.12.3 主题策略 - TopicPolicy
**职责**: 定义主题路由策略

---

### 3.13 AI聊天层 (ai_chat/)

#### 3.13.1 LLM服务 - LlmService
**职责**: 管理与LLM提供商的通信

**支持的提供商**:
- OpenAI
- Anthropic
- Gemini
- Groq
- DeepSeek
- MiniMax
- OpenRouter
- Ollama (本地)

#### 3.13.2 AI聊天气泡 - AiChatBubble
**职责**: 显示AI聊天消息

---

## 4. 关键类和函数

### 4.1 启动流程
```
main()
  ├─ 解析 --profile
  ├─ SingleApplication初始化
  ├─ DataHub元类型注册
  ├─ AppPaths::ensure_all()
  ├─ 数据库迁移
  ├─ 日志配置
  ├─ 主题应用
  ├─ Python环境检查
  │  ├─ needs_setup? → SetupScreen
  │  └─ 否则 → 直接MainWindow
  ├─ MCP初始化
  └─ 主事件循环
```

### 4.2 数据流
```
用户交互 → Screen → Service → HttpClient / PythonRunner → 数据获取
                                        ↓
                                   返回数据
                                        ↓
                              信号 → Screen更新UI
```

### 4.3 数据请求流程
```
用户点击
    ↓
Screen (MarketsScreen)
    ↓
Service (MarketDataService)
    ├─ 分支1: HTTP请求 (HttpClient)
    │       ↓
    │  解析JSON (QJsonDocument)
    │
    └─ 分支2: Python脚本 (PythonRunner)
            ↓
       启动Python子进程
            ↓
       脚本输出JSON到stdout
            ↓
       C++解析JSON响应
    ↓
发射信号 → Screen槽函数更新UI
```

### 4.4 Python集成流程
```
C++ (PythonRunner)
    ↓
QProcess启动Python解释器
    ↓
执行脚本 (scripts/Analytics/...)
    ↓
脚本输出JSON到stdout
    ↓
C++读取stdout，解析QJsonDocument
    ↓
返回数据给调用者
```

---

## 5. 依赖关系

### 5.1 核心依赖
| 依赖 | 版本 | 用途 |
| ---- | ---- | ---- |
| Qt6 | 6.8.3 | UI框架、图表、网络、数据库 |
| CMake | 3.27+ | 构建系统 |
| C++ | C++20 | 编程语言 |
| Python | 3.11+ | 数据分析脚本 |

### 5.2 第三方库
| 库 | 用途 | 获取方式 |
| --- | ---- | ------- |
| Qt Advanced Docking System | 停靠窗口 | FetchContent |
| SingleApplication | 单实例应用 | FetchContent |
| QGeoView | 地图组件 | FetchContent |
| QXlsx | Excel文件I/O | FetchContent (条件) |
| md4c | Markdown解析 | FetchContent |

### 5.3 Python依赖
Python需求文件位置:
- `fincept-qt/resources/requirements-numpy1.txt`
- `fincept-qt/resources/requirements-numpy2.txt`

**主要Python包**:
- numpy, pandas
- scipy, statsmodels
- quantstats, riskfolio-lib
- scikit-learn, xgboost
- tensorflow, pytorch
- yfinance, polars
- TA-Lib, technical indicators
- 等等...

---

## 6. 构建与部署

### 6.1 构建系统
**构建工具**: CMake 3.27+ + Ninja 1.11.1+

**CMake预设**:
- `win-release`: Windows 发布构建
- `win-debug`: Windows 调试构建
- `linux-release`: Linux 发布构建
- `linux-debug`: Linux 调试构建
- `macos-release`: macOS 发布构建
- `macos-debug`: macOS 调试构建

### 6.2 编译器要求
| 平台 | 编译器 | 最低版本 |
| ---- | ------ | -------- |
| Windows | MSVC | 19.38 (VS 2022 17.8+) |
| Linux | GCC | 12.3+ |
| macOS | Clang | 15.0+ (Xcode 15.2+) |

### 6.3 构建命令
```bash
cd fincept-qt

# 配置（使用预设）
cmake --preset linux-release

# 构建
cmake --build --preset linux-release

# 运行
./build/linux-release/FinceptTerminal
```

### 6.4 部署目标
| 平台 | 输出格式 | 部署方式 |
| ---- | ------- | ------- |
| Windows | .exe | 安装包 (NSIS) |
| macOS | .app | DMG |
| Linux | ELF | .run 安装包 |

### 6.5 环境变量
- `CMAKE_PREFIX_PATH`: Qt安装路径
- `FINCEPT_ALLOW_QT_DRIFT`: 允许Qt版本漂移（仅本地测试）

---

## 7. 开发指南

### 7.1 代码风格
- **C++**: 遵循 Google C++ Style Guide
- **命名**: 驼峰式 (camelCase), 类名首字母大写
- **缩进**: 4空格
- **格式**: 使用clang-format

### 7.2 添加新屏幕
1. 在`screens/`创建新的屏幕类
2. 继承`QWidget`或`IStatefulScreen`
3. 在`ScreenRouter`或`DockScreenRouter`中注册
4. 连接必要的服务信号

### 7.3 添加新服务
1. 在`services/`创建新的服务类
2. 使用单例模式或依赖注入
3. 提供Qt信号用于结果通知
4. 通过`EventBus`与其他组件通信

### 7.4 添加新Python脚本
1. 在`scripts/`相应目录创建
2. 输出JSON到stdout
3. 通过`PythonRunner`从C++调用
4. 处理返回的JSON结果

### 7.5 添加新经纪商
1. 继承`IBroker`接口
2. 实现所有纯虚函数
3. 在`BrokerRegistry`注册
4. 添加配置UI（如需要）

---

## 8. 测试与调试

### 8.1 日志系统
日志文件位置: `AppPaths::logs()/fincept.log`

日志标签:
- `App`: 应用程序
- `Network`: 网络请求
- `DB`: 数据库操作
- `Python`: Python脚本
- `Trading`: 交易相关
- `UI`: 用户界面

### 8.2 调试技巧
- 使用`FINCEPT_ALLOW_QT_DRIFT=ON`在本地测试不同Qt版本
- 启用`log/json_mode`获得结构化JSON日志
- 使用Qt Creator的调试器

### 8.3 测试命令
```bash
# 构建测试（如果启用）
cmake --preset linux-debug -DFINCEPT_BUILD_TESTS=ON
cmake --build --preset linux-debug

# 运行测试
cd build/linux-debug
ctest
```

---

## 9. 安全考虑

### 9.1 凭证存储
- 使用`SecureStorage`加密存储API密钥
- 不在代码中硬编码任何秘密
- 使用环境变量或配置文件（.env）

### 9.2 网络安全
- 所有API调用使用HTTPS
- Qt TLS加密
- WebSocket使用wss://

### 9.3 数据安全
- SQLite数据库文件权限控制
- 本地数据不包含原始密码
- 使用PIN锁定功能保护敏感操作

---

## 10. 性能优化

### 10.1 构建优化
- Unity Build（合并翻译单元）
- 编译器缓存（ccache/sccache）
- 发布构建使用LTO

### 10.2 运行时优化
- 线程模型：UI在主线程，后台工作在工作线程
- 数据库查询优化
- Python进程并发限制（默认3个）
- 数据缓存策略

---

## 11. 常见问题与故障排除

### 11.1 构建问题
**Q: 找不到Qt6**
A: 检查`CMAKE_PREFIX_PATH`是否指向Qt 6.8.3安装路径

**Q: MSVC版本不兼容**
A: 使用VS 2022 17.8+

**Q: macOS缺少AGL框架**
A: 项目已包含补丁，无需操作

### 11.2 运行时问题
**Q: Python脚本执行失败**
A: 检查Python环境是否正确设置，运行`SetupScreen`

**Q: 数据库打开失败**
A: 检查数据目录权限，删除`fincept.db-wal`和`fincept.db-shm`

**Q: MCP工具不工作**
A: 检查设置中的MCP服务器配置

---

## 12. 贡献指南

### 12.1 如何贡献
1. 阅读[docs/CONTRIBUTING.md](file:///workspace/docs/CONTRIBUTING.md)
2. 阅读[docs/CPP_CONTRIBUTOR_GUIDE.md](file:///workspace/docs/CPP_CONTRIBUTOR_GUIDE.md) 或 [docs/PYTHON_CONTRIBUTOR_GUIDE.md](file:///workspace/docs/PYTHON_CONTRIBUTOR_GUIDE.md)
3. Fork项目
4. 创建分支
5. 提交更改
6. 推送分支
7. 创建PR

### 12.2 代码审查清单
- [ ] 通过所有编译警告（CI中-Werror）
- [ ] 添加了必要的测试
- [ ] 更新了文档
- [ ] 遵循代码风格
- [ ] 没有引入安全问题
- [ ] 性能没有明显下降

---

## 13. 参考资料

### 13.1 内部文档
- [README.md](file:///workspace/README.md): 项目概述
- [docs/ARCHITECTURE.md](file:///workspace/docs/ARCHITECTURE.md): 架构文档
- [fincept-qt/DATAHUB_ARCHITECTURE.md](file:///workspace/fincept-qt/DATAHUB_ARCHITECTURE.md): DataHub架构
- [docs/CONTRIBUTING.md](file:///workspace/docs/CONTRIBUTING.md): 贡献指南

### 13.2 外部资源
- Qt6文档: https://doc.qt.io/qt-6/
- CMake文档: https://cmake.org/documentation/
- MCP规范: https://modelcontextprotocol.io/

---

## 附录

### A. 目录速查表
| 路径 | 说明 |
| ---- | ---- |
| /workspace/fincept-qt/src/app/ | 应用入口和主窗口 |
| /workspace/fincept-qt/src/core/ | 核心基础设施 |
| /workspace/fincept-qt/src/ui/ | UI组件和主题 |
| /workspace/fincept-qt/src/network/ | HTTP和WebSocket |
| /workspace/fincept-qt/src/storage/ | 数据库和仓库 |
| /workspace/fincept-qt/src/auth/ | 认证和安全 |
| /workspace/fincept-qt/src/python/ | Python集成 |
| /workspace/fincept-qt/src/trading/ | 交易引擎和经纪商 |
| /workspace/fincept-qt/src/services/ | 业务逻辑服务 |
| /workspace/fincept-qt/src/screens/ | UI屏幕 |
| /workspace/fincept-qt/src/mcp/ | MCP工具系统 |
| /workspace/fincept-qt/src/datahub/ | 数据分发系统 |
| /workspace/fincept-qt/src/ai_chat/ | AI聊天 |
| /workspace/fincept-qt/scripts/ | Python脚本 |

### B. 关键文件速查表
| 文件 | 说明 |
| ---- | ---- |
| fincept-qt/CMakeLists.txt | 构建配置 |
| fincept-qt/src/app/main.cpp | 主入口 |
| fincept-qt/src/app/MainWindow.h | 主窗口 |
| fincept-qt/src/core/config/AppConfig.h | 应用配置 |
| fincept-qt/src/python/PythonRunner.h | Python运行器 |
| fincept-qt/src/trading/BrokerInterface.h | 经纪商接口 |

### C. 许可信息
- 开源许可证: AGPL-3.0
- 商业许可证: 联系support@fincept.in

---

**文档版本**: 1.0  
**最后更新**: 2026年4月20日  
**项目版本**: Fincept Terminal v4.0.2
