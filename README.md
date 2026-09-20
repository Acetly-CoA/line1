graph TD
    classDef default fill:#f8f9fa,stroke:#ced4da,stroke-width:2px;
    classDef data fill:#e3f2fd,stroke:#1565c0,stroke-width:2px;
    classDef comp fill:#f3e5f5,stroke:#6a1b9a,stroke-width:2px;
    classDef mod fill:#fff3e0,stroke:#e65100,stroke-width:2px;
    classDef out fill:#e8f5e9,stroke:#2e7d32,stroke-width:2px;

    subgraph 模块一：表型与基因组数据预处理
        A1(两年田间表型数据):::data --> A3[空间校正与计算]:::comp
        A2(全基因组芯片测序):::data --> A4[基因型质控与插补]:::comp
    end

    subgraph 模块二：GWAS 降维与特征初筛
        A3 --> B1{交叉验证划分}:::comp
        A4 --> B1
        B1 --> B2[仅训练集执行 GWAS]:::comp
        B2 --> B3(获取 Top-K SNP 集合):::mod
    end

    subgraph 模块三：图卷积神经网络 GCN 构建
        B3 --> C1[定义节点 Nodes]:::mod
        B3 --> C2[定义边 Edges]:::mod
        C1 --> C3{构建基因组网络图}:::mod
        C2 --> C3
        C3 --> C4[多层图卷积网络]:::mod
        C4 --> C5((输出表型预测值)):::out
    end

    subgraph 模块四：模型评估
        C5 --> D1[对比经典模型]:::comp
        C5 --> D2[对比机器学习]:::comp
        D1 --> D3[评估准确率与RMSE]:::out
        D2 --> D3
    end

    subgraph 模块五：特征逆向解析与育种落地
        C4 -.可解释性分析.-> E1[权重溯源]:::comp
        E1 --> E2[挖掘核心 SNP]:::data
        E2 --> E3[开发功能型分子标记]:::out
        E3 --> E4(((育种群体田间验证))):::out
    end
