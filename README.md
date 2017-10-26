# borgbackup
Ansible role for borgbackup

#
# Set Vault Password for Protecting Data
#
echo "my_valut_secret_key" > .vault_pass


#
# Build SSH Key for Accessing Borg Repo on Rsync
#
ssh-keygen -t ed25519 -f id_ed25519_rsync_net -C borg_keypair


#
# Encrypt PassPhrase, Private & Public Key and add it to roles/borg_rsync/defaults/main.yml
#
echo 'my_secret_borg_encryption_key' | ansible-vault encrypt_string --stdin-name 'vault_borg_passphrase' >> roles/borg_rsync/defaults/main.yml
cat id_ed25519_rsync_net.pub | ansible-vault encrypt_string --stdin-name 'vault_borg_public_key' >> roles/borg_rsync/defaults/main.yml
cat id_ed25519_rsync_net | ansible-vault encrypt_string --stdin-name 'vault_borg_private_key' >> roles/borg_rsync/defaults/main.yml


#
# Remove SSH Keys
#
rm id_ed25519_rsync_net id_ed25519_rsync_net.pub


#
# Inventory
#
edit inventory file, add your hostsname
edit host-vars/hostname -> add your ip's

#
# Run
#
./borg_rsync.yml
