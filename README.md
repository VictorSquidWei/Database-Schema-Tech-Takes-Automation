# Database-Schema-Tech-Takes-Automation
erDiagram
    enterprise_wallets {
        SERIAL wallet_id PK
        VARCHAR wallet_address UK
        VARCHAR sportsbook_account_id
        DECIMAL total_bankroll
        DECIMAL btc_amount_usd
        DECIMAL btc_amount_btc
        DECIMAL eth_amount_usd
        DECIMAL eth_amount_eth
        DECIMAL usdt_amount_usd
        DECIMAL usdt_amount_usdt
        INTEGER total_users
        DECIMAL capacity_limit
        BOOLEAN is_active
        TIMESTAMP created_at
        TIMESTAMP updated_at
    }

    user_accounts {
        SERIAL user_id PK
        VARCHAR username UK
        VARCHAR email UK
        VARCHAR wallet_address
        DECIMAL total_deposited
        DECIMAL current_balance
        BOOLEAN is_active
        TIMESTAMP created_at
        TIMESTAMP updated_at
    }

    user_wallet_assignments {
        SERIAL assignment_id PK
        INTEGER user_id FK
        INTEGER wallet_id FK
        DECIMAL deposit_amount
        DECIMAL user_percentage
        VARCHAR cryptocurrency_type
        TIMESTAMP assigned_at
        BOOLEAN is_active
    }

    transactions {
        SERIAL transaction_id PK
        INTEGER user_id FK
        INTEGER wallet_id FK
        VARCHAR transaction_hash UK
        VARCHAR transaction_type
        DECIMAL amount
        VARCHAR cryptocurrency
        VARCHAR status
        BIGINT block_number
        DECIMAL gas_fee
        VARCHAR alchemy_webhook_id
        TIMESTAMP processed_at
        TIMESTAMP created_at
    }

    daily_balance_snapshots {
        SERIAL snapshot_id PK
        DATE snapshot_date
        INTEGER wallet_id FK
        INTEGER user_id FK
        DECIMAL balance_snapshot
        DECIMAL user_percentage
        DECIMAL wallet_total_value
        TIMESTAMP created_at
    }

    webhook_logs {
        SERIAL log_id PK
        VARCHAR webhook_id
        VARCHAR webhook_type
        JSONB payload
        VARCHAR status
        TIMESTAMP processed_at
        TEXT error_message
        TIMESTAMP created_at
    }

    %% Relationships
    user_accounts ||--o{ user_wallet_assignments : "has"
    enterprise_wallets ||--o{ user_wallet_assignments : "contains"
    user_accounts ||--o{ transactions : "performs"
    enterprise_wallets ||--o{ transactions : "processes"
    user_accounts ||--o{ daily_balance_snapshots : "tracks"
    enterprise_wallets ||--o{ daily_balance_snapshots : "monitors"
