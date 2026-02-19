```mermaid
graph TD
    classDef founder fill:#ff2d6e,stroke:#000,stroke-width:3px,color:#fff,font-weight:bold
    classDef dept1 fill:#00c6ff,stroke:#000,stroke-width:2px,color:#fff,font-weight:bold
    classDef dept2 fill:#00e08c,stroke:#000,stroke-width:2px,color:#000,font-weight:bold
    classDef dept3 fill:#ffb700,stroke:#000,stroke-width:2px,color:#000,font-weight:bold
    classDef dept4 fill:#9c27b0,stroke:#000,stroke-width:2px,color:#fff,font-weight:bold
    classDef dept5 fill:#ff5722,stroke:#000,stroke-width:2px,color:#fff,font-weight:bold
    classDef dept6 fill:#3f51b5,stroke:#000,stroke-width:2px,color:#fff,font-weight:bold
    classDef role fill:#ffffff,stroke:#444,stroke-width:1px,color:#222

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
