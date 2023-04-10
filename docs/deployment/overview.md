# Installation & Deployment

The deployment of OSISM is carried out in several successive phases.
The phases are documented in this chapter.

1. Preparation of a seed node
2. Bootstrap and deployment of a manager node
3. Provisioning of the bare-metal systems

    ```mermaid
    flowchart LR
        id1[1. seed] --> id2[2. manager] --> id3[3. baremetal]
    ```

4. Bootstrap of all remaining nodes
5. Deployment of the individual services

    ```mermaid
    flowchart LR
        id1[manager] --> id2[4. bootstrap] --> id3[5. services]
    ```
