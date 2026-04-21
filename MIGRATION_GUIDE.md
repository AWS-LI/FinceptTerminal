# 多源信息融合平台迁移指南

## 1. 项目概述

本迁移指南旨在指导如何将 Fincept Terminal 项目架构迁移到多源信息融合平台，实现光电、雷达、频谱等多源传感器数据的融合分析。

### 1.1 目标
- 利用 Fincept Terminal 现有的架构优势
- 构建一个功能完整的多源信息融合平台
- 实现传感器数据采集、时间同步、融合分析等核心功能

### 1.2 范围
- 迁移核心架构组件（DataHub、EventBus、PythonRunner、MCP等）
- 适配传感器数据采集和处理
- 实现融合算法集成
- 构建可视化和分析界面

## 2. 架构分析

### 2.1 Fincept Terminal 核心组件

| 组件 | 功能 | 迁移状态 |
|------|------|----------|
| DataHub | 统一数据管理和分发 | 直接迁移 |
| EventBus | 事件发布-订阅系统 | 直接迁移 |
| PythonRunner | Python脚本执行管理 | 直接迁移 |
| MCP | 工具调用系统 | 直接迁移 |
| 缓存系统 | 数据缓存和优化 | 直接迁移 |
| 日志系统 | 系统监控和调试 | 直接迁移 |
| 配置管理 | 系统参数设置 | 直接迁移 |

### 2.2 多源信息融合需求

| 需求 | 对应架构组件 | 实现方式 |
|------|-------------|----------|
| 传感器数据采集 | DataHub + 传感器生产者 | 开发传感器生产者 |
| 时间同步 | DataHub + 时间对齐模块 | 开发时间对齐模块 |
| 融合算法 | PythonRunner + 融合脚本 | 开发融合算法脚本 |
| 结果分析 | MCP + 分析工具 | 开发分析工具 |
| 可视化 | Qt界面组件 | 开发可视化组件 |

## 3. 迁移策略

### 3.1 分阶段迁移

| 阶段 | 任务 | 时间估计 |
|------|------|----------|
| 阶段1 | 基础架构搭建 | 2-3周 |
| 阶段2 | 传感器集成 | 2-4周 |
| 阶段3 | 融合算法实现 | 3-4周 |
| 阶段4 | 系统集成和测试 | 2-3周 |

### 3.2 技术选型

| 技术 | 版本 | 用途 |
|------|------|------|
| Qt | 6.x | 界面和核心架构 |
| C++ | C++17+ | 核心逻辑 |
| Python | 3.8+ | 融合算法 |
| SQLite | 3.x | 数据存储 |
| NumPy | 1.21+ | 科学计算 |
| OpenCV | 4.x | 图像处理（光电传感器） |

## 4. 具体迁移步骤

### 4.1 基础架构搭建

#### 4.1.1 环境配置
1. 安装Qt 6.x开发环境
2. 安装CMake 3.16+
3. 安装Python 3.8+和必要的库
4. 克隆Fincept Terminal代码仓库

#### 4.1.2 核心组件迁移
1. **DataHub迁移**
   - 复制DataHub相关文件到新项目
   - 修改主题命名规范，适配传感器数据
   - 配置数据刷新策略

2. **EventBus迁移**
   - 复制EventBus相关文件到新项目
   - 配置系统事件类型

3. **PythonRunner迁移**
   - 复制PythonRunner相关文件到新项目
   - 配置Python虚拟环境和依赖

4. **MCP迁移**
   - 复制MCP相关文件到新项目
   - 开发传感器和融合相关工具

### 4.2 传感器集成

#### 4.2.1 传感器生产者开发

```cpp
// 光学传感器生产者
class OpticalSensorProducer : public datahub::Producer {
public:
    OpticalSensorProducer(const QString& sensorId) : sensorId_(sensorId) {}
    
    QStringList topic_patterns() const override {
        return {
            QString("sensor:optical:%1:raw").arg(sensorId_),
            QString("sensor:optical:%1:processed").arg(sensorId_),
            QString("sensor:optical:%1:status").arg(sensorId_)
        };
    }
    
    void refresh(const QStringList& topics) override {
        for (const auto& topic : topics) {
            if (topic.contains(":raw")) {
                publishRawData();
            } else if (topic.contains(":processed")) {
                publishProcessedData();
            } else if (topic.contains(":status")) {
                publishStatus();
            }
        }
    }
    
private:
    void publishRawData() {
        // 采集原始数据
        OpticalData data = collectRawData();
        
        // 发布到DataHub
        datahub::DataHub::instance().publish(
            QString("sensor:optical:%1:raw").arg(sensorId_),
            QVariant::fromValue(data)
        );
    }
    
    QString sensorId_;
};

// 雷达传感器生产者
class RadarSensorProducer : public datahub::Producer {
    // 类似实现...
};

// 频谱传感器生产者
class SpectrumSensorProducer : public datahub::Producer {
    // 类似实现...
};
```

#### 4.2.2 时间同步模块

```cpp
class TimeAlignmentModule : public QObject {
public:
    TimeAlignmentModule() {
        // 订阅所有传感器数据
        datahub::DataHub::instance().subscribe_pattern(
            this, 
            "sensor:*:raw", 
            [this](const QString& topic, const QVariant& data) {
                processSensorData(topic, data);
            }
        );
    }
    
    void processSensorData(const QString& topic, const QVariant& data) {
        // 提取传感器数据
        auto sensorData = data.value<SensorData>();
        
        // 按时间窗口分组
        QDateTime windowKey = alignToTimeWindow(sensorData.timestamp);
        sensorDataByWindow_[windowKey].append(sensorData);
        
        // 当窗口数据足够时，执行时间对齐
        if (hasEnoughData(windowKey)) {
            alignData(windowKey);
        }
    }
    
    void alignData(const QDateTime& window) {
        // 执行时间对齐算法
        auto alignedData = performTimeAlignment(sensorDataByWindow_[window]);
        
        // 发布对齐后的数据
        datahub::DataHub::instance().publish(
            "fusion:aligned_data", 
            QVariant::fromValue(alignedData)
        );
        
        // 清理窗口数据
        sensorDataByWindow_.remove(window);
    }
    
private:
    QHash<QDateTime, QVector<SensorData>> sensorDataByWindow_;
};
```

### 4.3 融合算法实现

#### 4.3.1 Python融合算法

```python
# fusion_algorithms/multi_source_fusion.py
import numpy as np
from scipy import stats

class FusionAlgorithm:
    def __init__(self, params):
        self.params = params
    
    def fuse(self, sensor_data):
        """多源数据融合算法"""
        # 提取不同传感器的数据
        optical_data = []
        radar_data = []
        spectrum_data = []
        
        for data in sensor_data:
            sensor_type = data['sensor_type']
            if sensor_type == 'optical':
                optical_data.append(data['value'])
            elif sensor_type == 'radar':
                radar_data.append(data['value'])
            elif sensor_type == 'spectrum':
                spectrum_data.append(data['value'])
        
        # 数据预处理
        optical_processed = self.preprocess_optical(optical_data)
        radar_processed = self.preprocess_radar(radar_data)
        spectrum_processed = self.preprocess_spectrum(spectrum_data)
        
        # 执行融合
        fusion_result = self.perform_fusion(
            optical_processed, 
            radar_processed, 
            spectrum_processed
        )
        
        return fusion_result
    
    def preprocess_optical(self, data):
        # 光学数据预处理
        return np.array(data)
    
    def preprocess_radar(self, data):
        # 雷达数据预处理
        return np.array(data)
    
    def preprocess_spectrum(self, data):
        # 频谱数据预处理
        return np.array(data)
    
    def perform_fusion(self, optical, radar, spectrum):
        # 加权平均融合
        weights = self.params.get('weights', [0.3, 0.4, 0.3])
        
        # 计算各传感器数据的均值
        optical_mean = np.mean(optical) if len(optical) > 0 else 0
        radar_mean = np.mean(radar) if len(radar) > 0 else 0
        spectrum_mean = np.mean(spectrum) if len(spectrum) > 0 else 0
        
        # 加权融合
        fused_value = (
            weights[0] * optical_mean +
            weights[1] * radar_mean +
            weights[2] * spectrum_mean
        )
        
        # 计算融合置信度
        confidence = self.calculate_confidence(optical, radar, spectrum)
        
        return {
            'value': fused_value,
            'confidence': confidence,
            'sensor_contributions': {
                'optical': optical_mean,
                'radar': radar_mean,
                'spectrum': spectrum_mean
            }
        }
    
    def calculate_confidence(self, optical, radar, spectrum):
        # 计算融合置信度
        confidences = []
        if len(optical) > 0:
            confidences.append(1.0 - np.std(optical) / np.mean(optical) if np.mean(optical) > 0 else 0)
        if len(radar) > 0:
            confidences.append(1.0 - np.std(radar) / np.mean(radar) if np.mean(radar) > 0 else 0)
        if len(spectrum) > 0:
            confidences.append(1.0 - np.std(spectrum) / np.mean(spectrum) if np.mean(spectrum) > 0 else 0)
        
        return np.mean(confidences) if confidences else 0

def run_fusion(sensor_data, params):
    """运行融合算法"""
    algorithm = FusionAlgorithm(params)
    result = algorithm.fuse(sensor_data)
    return result

if __name__ == '__main__':
    import json
    import sys
    
    # 读取输入参数
    input_data = json.loads(sys.argv[1])
    sensor_data = input_data.get('sensor_data', [])
    params = input_data.get('params', {})
    
    # 运行融合
    result = run_fusion(sensor_data, params)
    
    # 输出结果
    print(json.dumps(result))
```

#### 4.3.2 融合引擎

```cpp
class FusionEngine : public QObject {
public:
    FusionEngine() {
        // 订阅对齐后的数据
        datahub::DataHub::instance().subscribe(
            this, 
            "fusion:aligned_data", 
            [this](const QVariant& data) {
                performFusion(data);
            }
        );
    }
    
    void performFusion(const QVariant& data) {
        // 提取对齐后的数据
        auto alignedData = data.value<QVector<SensorData>>();
        
        // 准备算法参数
        QJsonArray sensorDataArray;
        for (const auto& sensor : alignedData) {
            QJsonObject sensorObj;
            sensorObj["sensor_type"] = sensor.sensorType;
            sensorObj["value"] = sensor.value;
            sensorObj["timestamp"] = sensor.timestamp.toMSecsSinceEpoch();
            sensorObj["confidence"] = sensor.confidence;
            sensorDataArray.append(sensorObj);
        }
        
        QJsonObject params;
        params["weights"] = QJsonArray{0.3, 0.4, 0.3};
        params["algorithm"] = "weighted_average";
        
        QJsonObject input;
        input["sensor_data"] = sensorDataArray;
        input["params"] = params;
        
        QString args = QJsonDocument(input).toJson(QJsonDocument::Compact);
        
        // 执行Python融合算法
        python::PythonRunner::instance().run(
            "fusion_algorithms/multi_source_fusion.py",
            {args},
            [this](const python::PythonResult& result) {
                if (result.success) {
                    processFusionResult(result.output);
                } else {
                    LOG_ERROR("Fusion", "Algorithm failed: " + result.error);
                }
            }
        );
    }
    
    void processFusionResult(const QString& output) {
        // 解析融合结果
        QJsonDocument doc = QJsonDocument::fromJson(output.toUtf8());
        if (!doc.isObject()) {
            LOG_ERROR("Fusion", "Invalid fusion result format");
            return;
        }
        
        QJsonObject resultObj = doc.object();
        FusionResult result;
        result.value = resultObj["value"].toDouble();
        result.confidence = resultObj["confidence"].toDouble();
        
        // 发布融合结果
        datahub::DataHub::instance().publish(
            "fusion:result", 
            QVariant::fromValue(result)
        );
    }
};
```

### 4.4 MCP工具开发

#### 4.4.1 传感器管理工具

```cpp
std::vector<ToolDef> get_sensor_tools() {
    std::vector<ToolDef> tools;
    
    // 传感器配置工具
    {
        ToolDef t;
        t.name = "sensor_configure";
        t.description = "配置传感器参数";
        t.category = "sensor";
        t.input_schema.properties = QJsonObject{
            {"sensor_id", QJsonObject{{"type", "string"}, {"description", "传感器ID"}}},
            {"params", QJsonObject{{"type", "object"}, {"description", "传感器参数"}}}
        };
        t.input_schema.required = {"sensor_id"};
        t.handler = [](const QJsonObject& args) -> ToolResult {
            QString sensorId = args["sensor_id"].toString();
            QJsonObject params = args["params"].toObject();
            
            // 实现传感器配置逻辑
            SensorManager::instance().configureSensor(sensorId, params);
            
            return ToolResult::ok(QString("传感器 %1 配置成功").arg(sensorId));
        };
        tools.push_back(std::move(t));
    }
    
    // 传感器状态查询工具
    {
        ToolDef t;
        t.name = "sensor_status";
        t.description = "获取传感器状态";
        t.category = "sensor";
        t.input_schema.properties = QJsonObject{
            {"sensor_id", QJsonObject{{"type", "string"}, {"description", "传感器ID"}}}
        };
        t.input_schema.required = {"sensor_id"};
        t.handler = [](const QJsonObject& args) -> ToolResult {
            QString sensorId = args["sensor_id"].toString();
            
            // 获取传感器状态
            auto status = SensorManager::instance().getSensorStatus(sensorId);
            
            return ToolResult::ok_data(QJsonObject{
                {"sensor_id", sensorId},
                {"status", status.status},
                {"last_update", status.lastUpdate.toMSecsSinceEpoch()},
                {"battery", status.battery},
                {"signal_strength", status.signalStrength}
            });
        };
        tools.push_back(std::move(t));
    }
    
    return tools;
}
```

#### 4.4.2 融合算法工具

```cpp
std::vector<ToolDef> get_fusion_tools() {
    std::vector<ToolDef> tools;
    
    // 运行融合算法工具
    {
        ToolDef t;
        t.name = "run_fusion_algorithm";
        t.description = "执行多源信息融合算法";
        t.category = "fusion";
        t.input_schema.properties = QJsonObject{
            {"algorithm", QJsonObject{{"type", "string"}, {"description", "融合算法名称"}}},
            {"sensor_data", QJsonObject{{"type", "array"}, {"description", "传感器数据"}}},
            {"params", QJsonObject{{"type", "object"}, {"description", "算法参数"}}}
        };
        t.input_schema.required = {"sensor_data"};
        t.handler = [](const QJsonObject& args) -> ToolResult {
            QString algorithm = args["algorithm"].toString("weighted_average");
            QJsonArray sensorData = args["sensor_data"].toArray();
            QJsonObject params = args["params"].toObject();
            
            // 执行融合算法
            auto result = FusionService::instance().runAlgorithm(algorithm, sensorData, params);
            
            return ToolResult::ok_data(result);
        };
        tools.push_back(std::move(t));
    }
    
    // 融合结果分析工具
    {
        ToolDef t;
        t.name = "analyze_fusion_result";
        t.description = "分析融合结果";
        t.category = "fusion";
        t.input_schema.properties = QJsonObject{
            {"fusion_result", QJsonObject{{"type", "object"}, {"description", "融合结果"}}}
        };
        t.input_schema.required = {"fusion_result"};
        t.handler = [](const QJsonObject& args) -> ToolResult {
            QJsonObject fusionResult = args["fusion_result"].toObject();
            
            // 分析融合结果
            auto analysis = FusionService::instance().analyzeResult(fusionResult);
            
            return ToolResult::ok_data(analysis);
        };
        tools.push_back(std::move(t));
    }
    
    return tools;
}
```

## 5. 系统集成

### 5.1 主应用集成

```cpp
int main(int argc, char *argv[]) {
    QApplication app(argc, argv);
    
    // 初始化核心组件
    core::Logger::instance().initialize();
    datahub::DataHub::instance().initialize();
    mcp::McpService::instance().initialize();
    
    // 注册传感器生产者
    registerSensorProducers();
    
    // 初始化融合引擎
    auto fusionEngine = new FusionEngine();
    
    // 初始化时间对齐模块
    auto timeAlignment = new TimeAlignmentModule();
    
    // 注册MCP工具
    registerMcpTools();
    
    // 创建主窗口
    MainWindow mainWindow;
    mainWindow.show();
    
    return app.exec();
}
```

### 5.2 界面设计

#### 5.2.1 主界面布局
- **传感器状态面板**：显示各传感器状态
- **数据可视化面板**：实时显示传感器数据
- **融合结果面板**：显示融合结果和分析
- **控制面板**：传感器控制和算法参数调整

#### 5.2.2 数据可视化
- **实时曲线图**：显示传感器数据和融合结果
- **3D可视化**：融合结果的空间分布
- **热力图**：数据密度和异常分布

## 6. 测试计划

### 6.1 单元测试
- **传感器数据采集测试**：验证传感器数据正确采集
- **时间同步测试**：验证多源数据时间对齐
- **融合算法测试**：验证融合结果准确性
- **MCP工具测试**：验证工具调用功能

### 6.2 集成测试
- **系统集成测试**：验证各组件协同工作
- **性能测试**：验证系统响应时间和吞吐量
- **稳定性测试**：验证系统长时间运行稳定性

### 6.3 验收测试
- **功能验收**：验证所有功能正常工作
- **性能验收**：验证系统满足性能要求
- **用户验收**：验证用户体验符合预期

## 7. 部署方案

### 7.1 开发环境部署
- **Qt开发环境**：Qt 6.x
- **C++编译器**：支持C++17
- **Python环境**：Python 3.8+，安装必要的库
- **构建系统**：CMake 3.16+

### 7.2 生产环境部署
- **Windows**：使用NSIS创建安装程序
- **Linux**：使用AppImage或deb包
- **macOS**：使用DMG或PKG格式

### 7.3 依赖管理
- **Qt依赖**：打包Qt运行时库
- **Python依赖**：打包虚拟环境和必要的库
- **第三方库**：打包OpenCV等必要的库

## 8. 维护指南

### 8.1 系统监控
- **日志系统**：定期检查系统日志
- **性能监控**：监控系统资源使用
- **传感器状态**：监控传感器健康状态

### 8.2 故障排查
- **传感器故障**：检查传感器连接和状态
- **融合算法问题**：检查算法参数和输入数据
- **系统性能问题**：检查资源使用和瓶颈

### 8.3 系统更新
- **传感器固件更新**：定期更新传感器固件
- **融合算法更新**：更新融合算法和参数
- **系统组件更新**：更新系统核心组件

## 9. 附录

### 9.1 传感器数据格式

```cpp
// 传感器数据结构
struct SensorData {
    QString sensorId;
    QString sensorType; // optical, radar, spectrum
    QDateTime timestamp;
    double value;
    double confidence;
    QVariantMap metadata;
};

// 融合结果结构
struct FusionResult {
    double value;
    double confidence;
    QDateTime timestamp;
    QVariantMap sensorContributions;
};
```

### 9.2 DataHub主题规范

| 主题 | 描述 | 数据类型 |
|------|------|----------|
| sensor:optical:<id>:raw | 光学传感器原始数据 | SensorData |
| sensor:optical:<id>:processed | 光学传感器处理数据 | SensorData |
| sensor:radar:<id>:raw | 雷达传感器原始数据 | SensorData |
| sensor:radar:<id>:processed | 雷达传感器处理数据 | SensorData |
| sensor:spectrum:<id>:raw | 频谱传感器原始数据 | SensorData |
| sensor:spectrum:<id>:processed | 频谱传感器处理数据 | SensorData |
| sensor:*:status | 传感器状态 | SensorStatus |
| fusion:aligned_data | 时间对齐后的数据 | QVector<SensorData> |
| fusion:result | 融合结果 | FusionResult |
| fusion:analysis | 融合结果分析 | FusionAnalysis |

### 9.3 MCP工具列表

| 工具名称 | 功能 | 类别 |
|----------|------|------|
| sensor_configure | 配置传感器参数 | sensor |
| sensor_status | 获取传感器状态 | sensor |
| sensor_calibrate | 校准传感器 | sensor |
| run_fusion_algorithm | 执行融合算法 | fusion |
| analyze_fusion_result | 分析融合结果 | fusion |
| time_align_data | 时间对齐数据 | fusion |
| system_status | 获取系统状态 | system |
| log_analyzer | 分析系统日志 | system |

## 10. 总结

本迁移指南提供了将Fincept Terminal架构迁移到多源信息融合平台的详细步骤和实现方案。通过合理的迁移策略和模块化设计，可以构建一个功能完整、性能优化的多源信息融合系统。

关键成功因素包括：
- 充分利用Fincept Terminal的现有架构优势
- 合理设计传感器数据采集和处理流程
- 实现高效的时间同步和融合算法
- 构建用户友好的界面和分析工具

通过本指南的实施，可以快速构建一个满足多源信息融合需求的系统，为各种应用场景提供强大的决策支持。