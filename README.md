# update-Storage-Node-og-Labs
🔩 Updates OGLabs

🟢Storage Node (Turbo) Updates
cd 0g-storage-node/run/db
rm -r data_db 
sudo systemctl daemon-reload && \
sudo systemctl enable zgs && \
sudo systemctl restart zgs && \
sudo systemctl status zgs


🟢Update & Migrate Storage Node (Turbo => Standard) V0.8.6


rm -r 0g-storage-node
git clone -b v0.8.6 https://github.com/0glabs/0g-storage-node.git
cd $HOME/0g-storage-node
git stash
git fetch --all --tags
git checkout 67a8241
git submodule update --init
cargo build --release
rm -rf $HOME/0g-storage-node/run/config.toml
curl -o $HOME/0g-storage-node/run/config.toml https://raw.githubusercontent.com/zstake-xyz/test/refs/heads/main/0g_storage_config.toml
nano $HOME/0g-storage-node/run/config.toml

Paste Private Key "miner_key"

CTRL X, Y, Enter

sudo tee /etc/systemd/system/zgs.service > /dev/null <<EOF
[Unit]
Description=ZGS Node
After=network.target

[Service]
User=$USER
WorkingDirectory=$HOME/0g-storage-node/run
ExecStart=$HOME/0g-storage-node/target/release/zgs_node --config $HOME/0g-storage-node/run/config.toml
Restart=on-failure
RestartSec=10
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target
EOF
sudo systemctl daemon-reload && sudo systemctl enable zgs && sudo systemctl start zgs
tail -f ~/0g-storage-node/run/log/zgs.log.$(TZ=UTC date +%Y-%m-%d)


🟢Recommend RPC :
      
https://evmrpc-testnet.0g.ai
https://evm-rpc.0g.testnet.node75.org


