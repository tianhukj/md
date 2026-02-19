```mermaid
graph LR
    classDef founder fill:#fce4ec,stroke:#880e4f,stroke-width:2px,color:#880e4f,font-weight:bold
    classDef dept1 fill:#e3f2fd,stroke:#0d47a1,stroke-width:1px,color:#0d47a1,font-weight:bold
    classDef dept2 fill:#e8f5e8,stroke:#1b5e20,stroke-width:1px,color:#1b5e20,font-weight:bold
    classDef dept3 fill:#fff8e1,stroke:#ff8f00,stroke-width:1px,color:#ff8f00,font-weight:bold
    classDef dept4 fill:#f3e5f5,stroke:#4a148c,stroke-width:1px,color:#4a148c,font-weight:bold
    classDef dept5 fill:#fbe9e7,stroke:#bf360c,stroke-width:1px,color:#bf360c,font-weight:bold
    classDef dept6 fill:#ede7f6,stroke:#311b92,stroke-width:1px,color:#311b92,font-weight:bold
    classDef role fill:#ffffff,stroke:#9e9e9e,stroke-width:1px,color:#212121

    A[创始人 Founder<br>CEO / CSO / P4实验室主任 / BSO]:::founder

    A --> B[管理层]:::dept1
    B --> B1[总经理 CEO]:::role
    B --> B2[首席科学官 CSO]:::role
    B --> B3[质量负责人 QD]:::role
    B --> B4[P4实验室主任]:::role

    A --> C[研发检测部]:::dept2
    C --> C1[研发主管]:::role
    C --> C2[研发工程师]:::role
    C --> C3[检测工程师]:::role
    C --> C4[实验员]:::role

    A --> D[质量合规部]:::dept3
    D --> D1[质量负责人 QD]:::role
    D --> D2[质量保证 QA]:::role
    D --> D3[质量控制 QC]:::role
    D --> D4[生物安全负责人]:::role

    A --> E[P4实验室专属]:::dept4
    E --> E1[P4实验室主管]:::role
    E --> E2[高级生物安全专员]:::role
    E --> E3[灭菌消杀专员]:::role
    E --> E4[防护装备管理员]:::role

    A --> F[市场商务部]:::dept5
    F --> F1[商务总监 BD]:::role
    F --> F2[销售代表]:::role
    F --> F3[市场专员]:::role

    A --> G[行政财务部]:::dept6
    G --> G1[行政主管]:::role
    G --> G2[人事专员]:::role
    G --> G3[会计/出纳]:::role
    G --> G4[采购专员]:::role
