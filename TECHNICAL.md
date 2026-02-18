# 🔧 Technical Overview - BrainrotVerse

Documento técnico sobre a stack e arquitetura do projeto.

## Tech Stack

### Frontend (Client)
- **Language**: Luau (Roblox native scripting)
- - **Framework**: Roblox GUI (FastFlags para otimização)
  - - **State Management**: RemoteEvents/RemoteFunctions
    - - **UI Library**: Custom (será desenvolvido)
     
      - ### Backend (Server)
      - - **Language**: Luau (Roblox native)
        - - **Database**: Roblox DataStore API (Cloud-based)
          - - **Architecture**: Service-based (modular)
            - - **Networking**: RemoteEvents (unreliable) + RemoteFunctions (reliable)
             
              - ### Infrastructure
              - - **Hosting**: Roblox Servers (managed)
                - - **Scalability**: Auto-routing até 100 players/server (Roblox default)
                  - - **Regional**: NA, EU, LATAM (geolocation)
                    - - **Version Control**: Git + GitHub
                     
                      - ## Arquitetura do Projeto
                     
                      - ```
                        BrainrotVerse/
                        ├── src/
                        │   ├── client/           # Client-side code (LocalScripts)
                        │   │   ├── UI/
                        │   │   │   ├── BattleUI.lua
                        │   │   │   ├── MainHub.lua
                        │   │   │   └── CreatureDisplay.lua
                        │   │   ├── Controllers/
                        │   │   │   ├── BattleController.lua
                        │   │   │   ├── CreatureController.lua
                        │   │   │   └── InputHandler.lua
                        │   │   └── Constants/
                        │   │       └── Config.lua
                        │   │
                        │   ├── server/           # Server-side code (Scripts)
                        │   │   ├── Services/
                        │   │   │   ├── PlayerService.lua
                        │   │   │   ├── CreatureService.lua
                        │   │   │   ├── BattleService.lua
                        │   │   │   └── DataStore Service.lua
                        │   │   ├── Combat/
                        │   │   │   ├── BattleEngine.lua
                        │   │   │   ├── Damage.lua
                        │   │   │   └── Effects.lua
                        │   │   └── Database/
                        │   │       ├── Schemas.lua
                        │   │       └── Migrations.lua
                        │   │
                        │   └── shared/           # Shared utilities
                        │       ├── Types/
                        │       │   └── Types.luau
                        │       ├── Constants/
                        │       │   └── GameConstants.lua
                        │       └── Utils/
                        │           ├── TableUtils.lua
                        │           ├── MathUtils.lua
                        │           └── EventUtils.lua
                        │
                        ├── docs/                 # Documentation
                        │   ├── API.md
                        │   ├── DATABASE.md
                        │   └── CONTRIBUTING.md
                        │
                        └── .github/
                            ├── workflows/        # CI/CD (future)
                            └── ISSUE_TEMPLATE/
                        ```

                        ## Data Flow

                        ```
                        Player Input (Client)
                                ↓
                        LocalScript → RemoteEvent (request)
                                ↓
                        Server Script (validates)
                                ↓
                        Service (executes logic)
                                ↓
                        DataStore (persists)
                                ↓
                        RemoteEvent (response) → LocalScript
                                ↓
                        UI Update (Client)
                        ```

                        ## Key Services

                        ### PlayerService
                        - Gerencia dados do jogador
                        - - Handles joins/leaves
                          - - Syncs com DataStore
                           
                            - ### CreatureService
                            - - CRUD para criaturas
                              - - Evolution logic
                                - - Party management
                                 
                                  - ### BattleService
                                  - - Matchmaking
                                    - - Turn management
                                      - - Damage calculation
                                        - - Win/loss handling
                                         
                                          - ### DataStoreService
                                          - - Wrapper seguro para DataStore
                                            - - Cache management
                                              - - Backup strategy
                                               
                                                - ## Database Schema
                                               
                                                - ```lua
                                                  -- PlayerData
                                                  {
                                                    userId = 123,
                                                    username = "PlayerName",
                                                    level = 1,
                                                    exp = 0,
                                                    robux = 0,
                                                    creatures = {} -- array de IDs,
                                                    partyIndex = 1,
                                                    lastLogin = 0,
                                                    joinDate = 0
                                                  }

                                                  -- CreatureData
                                                  {
                                                    creatureId = "uuid",
                                                    dexId = 1, -- reference to dex
                                                    level = 1,
                                                    exp = 0,
                                                    hp = 100,
                                                    stats = {
                                                      attack = 10,
                                                      defense = 8,
                                                      spAtk = 9,
                                                      spDef = 7,
                                                      speed = 11
                                                    },
                                                    moves = {1, 2, 3, 4} -- move IDs,
                                                    ownerId = 123
                                                  }
                                                  ```

                                                  ## Performance Targets

                                                  - **Load Time**: < 3 segundos
                                                  - - **Battle Latency**: < 200ms
                                                    - - **Server Tick Rate**: 30 Hz
                                                      - - **DataStore Calls**: < 50/min (Roblox limit)
                                                        - - **Concurrent Players**: 500+ (com multi-server)
                                                         
                                                          - ## Development Workflow
                                                         
                                                          - 1. **Feature Branch**: `feature/creature-system`
                                                            2. 2. **Development**: Code locally ou direto em Studio
                                                               3. 3. **Testing**: Playtests no servidor de dev
                                                                  4. 4. **PR**: Pull request para `develop`
                                                                     5. 5. **Merge**: Code review + merge
                                                                        6. 6. **Release**: `develop` → `main`
                                                                          
                                                                           7. ## Tools & Dependencies
                                                                          
                                                                           8. - **GitHub**: Version control
                                                                              - - **Roblox Studio**: IDE principal
                                                                                - - **Rojo** (opcional): CLI para syncing
                                                                                  - - **Luau LSP**: IntelliSense (VSCode)
                                                                                   
                                                                                    - ## Segurança
                                                                                   
                                                                                    - - **Client-Side**: Nunca confiar em validações client
                                                                                      - - **Server-Side**: Validate TUDO
                                                                                        - - **DataStore**: Keys com prefixo (e.g., "player_123")
                                                                                          - - **Exploits**: Anti-cheat básico (futuro)
                                                                                           
                                                                                            - ## Roadmap Técnico
                                                                                           
                                                                                            - - **MVP (Mar)**: Core systems estáveis
                                                                                              - - **Alpha (Apr)**: Monetização + Events
                                                                                                - - **Beta (May)**: Aura system, Raids
                                                                                                  - - **Launch (Jun)**: Polish + Marketing push
                                                                                                   
                                                                                                    - ---
                                                                                                    
                                                                                                    **Last Updated**: 18/02/2026
                                                                                                    **Maintainer**: @Sayed, @Blakes
