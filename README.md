```mermaid
graph TD
    %% BioRender 风格全局与节点类定义
    classDef default fill:#f8f9fa,stroke:#ced4da,stroke-width:2px,rx:12px,ry:12px,color:#343a40;
    classDef data_node fill:#e3f2fd,stroke:#64b5f6,stroke-width:2px,rx:12px,ry:12px,color:#1565c0,font-weight:bold;
    classDef compute_node fill:#f3e5f5,stroke:#ba68c8,stroke-width:2px,rx:12px,ry:12px,color:#6a1b9a,font-weight:bold;
    classDef model_node fill:#fff3e0,stroke:#ffb74d,stroke-width:2px,rx:12px,ry:12px,color:#e65100,font-weight:bold;
    classDef output_node fill:#e8f5e9,stroke:#81c784,stroke-width:2px,rx:12px,ry:12px,color:#2e7d32,font-weight:bold;

    subgraph 模块一：表型与基因组数据预处理
        A1(🌾 两年田间表型数据):::data_node
        A2(🧬 全基因组芯片测序):::data_node
        A3[💻 空间校正与 BLUP/BLUE 计算]:::compute_node
        A4[💻 基因型质控与 Imputation 插补]:::compute_node
        
        A1 --> A3
        A2 --> A4
    end

    subgraph 模块二：GWAS 降维与特征初筛
        B1{防数据泄露: 交叉验证划分}:::compute_node
        B2[🔬 仅训练集执行 GWAS 分析]:::compute_node
        B3(🎯 获取 Top-K 强相关 SNP 集合):::model_node

        A3 --> B1
        A4 --> B1
        B1 --> B2
        B2 --> B3
    end

    subgraph 模块三：图卷积神经网络 GCN 构建
        C1[🔵 定义节点 Nodes: SNP 剂量矩阵]:::model_node
        C2[🔗 定义边 Edges: LD / 物理距离]:::model_node
        C3{🕸️ 构建基因组网络图 Graph}:::model_node
        C4[🧠 多层图卷积特征提取网络]:::model_node
        C5((📊 输出表型预测值)):::output_node

        B3 --> C1
        B3 --> C2
        C1 --> C3
        C2 --> C3
        C3 --> C4
        C4 --> C5
    end

    subgraph 模块四：模型评估与 Benchmark
        D1[📈 经典模型: GBLUP / rrBLUP]:::compute_node
        D2[📈 机器学习: XGBoost / RF]:::compute_node
        D3[🏆 评估指标: 准确率 r / RMSE]:::output_node

        C5 --> D1
        C5 --> D2
        D1 --> D3
        D2 --> D3
    end

    subgraph 模块五：特征逆向解析与育种落地
        E1[🔍 GNNExplainer/SHAP 权重溯源]:::compute_node
        E2[🧬 挖掘核心 SNP 与上位性互作]:::data_node
        E3[🧪 开发功能型分子标记 KASP]:::output_node
        E4(((🌱 育种群体田间真实应用与验证))):::output_node

        C4 -.可解释性分析.-> E1
        E1 --> E2
        E2 --> E3
        E3 --> E4
    end
```
