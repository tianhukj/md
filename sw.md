```mermaid
graph TD
    %% 定义样式类
    classDef founder fill:#ff6b6b,stroke:#333,stroke-width:2px,color:white,font-weight:bold
    classDef dept fill:#4ecdc4,stroke:#333,stroke-width:1px,color:white,font-weight:bold
    classDef role fill:#f7fff7,stroke:#333,stroke-width:1px,color:#333

    %% 创始人节点
    A[创始人 Founder<br>法定代表人 / CEO / CSO / P4实验室主任 / BSO]:::founder

    %% 一级部门
    A --> B[管理层]:::dept
    A --> C[研发检测部]:::dept
    A --> D[质量合规部]:::dept
    A --> E[P4实验室专属]:::dept
    A --> F[市场商务部]:::dept
    A --> G[行政财务部]:::dept

    %% 管理层
    B --> B1[总经理 CEO]:::role
    B --> B2[首席科学官 CSO]:::role
    B --> B3[质量负责人 QD]:::role
    B --> B4[P4实验室主任]:::role

    %% 研发检测部
    C --> C1[研发主管]:::role
    C --> C2[研发工程师]:::role
    C --> C3[检测工程师]:::role
    C --> C4[实验员]:::role

    %% 质量合规部
    D --> D1[质量负责人 QD]:::role
    D --> D2[质量保证 QA]:::role
    D --> D3[质量控制 QC]:::role
    D --> D4[生物安全负责人]:::role

    %% P4实验室专属
    E --> E1[P4实验室主管]:::role
    E --> E2[高级生物安全专员]:::role
    E --> E3[灭菌消杀专员]:::role
    E --> E4[防护装备管理员]:::role

    %% 市场商务部
    F --> F1[商务总监 BD]:::role
    F --> F2[销售代表]:::role
    F --> F3[市场专员]:::role

    %% 行政财务部
    G --> G1[行政主管]:::role
    G --> G2[人事专员]:::role
    G --> G3[会计/出纳]:::role
    G --> G4[采购专员]:::role
    F --> F3[市场专员]
    
    G --> G1[行政主管]
    G --> G2[人事专员]
    G --> G3[会计/出纳]
    G --> G4[采购专员]

    G --> G1[行政主管]
    G --> G2[人事专员]
    G --> G3[会计/出纳]
    G --> G4[采购专员]
    F --> F3[市场专员]
    
    G --> G1[行政主管]
    G --> G2[人事专员]
    G --> G3[会计/出纳]
    G --> G4[采购专员]
