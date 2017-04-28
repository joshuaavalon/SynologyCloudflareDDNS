# Synology Cloudflare DDNS Script 📜
The is a script to be used to add [Cloudflare](https://www.cloudflare.com/) as a DDNS to [Synology](https://www.synology.com/) NAS. This is a modified version [script](https://gist.github.com/tehmantra/f1d2579f3c922e8bb4a0) from [Michael Wildman](https://gist.github.com/tehmantra). The script used an updated API, Cloudflare API v4.

## How to use
### Access Synology via SSH
1. Login to your DSM
2. Go to Control Panel > Terminal & SNMP > Enable SSH service
3. Use your client or commandline to access Synology. If you don't have any, I recommand you can try out [MobaXterm](http://mobaxterm.mobatek.net/) for Windows.
4. Use your Synology admin account to connect.

### Run commands in Synology
1. Download `cloudflareddns.sh` from this repository to `/sbin/cloudflaredns.sh`
```
wget https://raw.githubusercontent.com/joshuaavalon/SynologyCloudflareDDNS/master/cloudflareddns.sh -O /sbin/cloudflaredns.sh
```
It is not a must, you can put I whatever you want. If you put the script in other name or path, make sure you use the right path.

2. Give others execute permission
```
chmod +x /sbin/cloudflaredns.sh
```

3. Add `cloudflareddns.sh` to Synology
```
cat >> /etc.defaults/ddns_provider.conf << 'EOF'
[Cloudflare]
        modulepath=/sbin/cloudflaredns.sh
        queryurl=https://www.cloudflare.com/
E*.
```
`queryurl` does not matter because we are going to use our script but it is needed.

### Get Cloudflare parameters
1. Go to your domain overview page and get the **Zone ID**.
2. Go to your [account setting page](https://www.cloudflare.com/a/account/my-account) and get **API Key**.
3. Get record id using Cloudflare API.
```
curl -X GET "https://api.cloudflare.com/client/v4/zones/[Zone ID]" \
     -H "X-Auth-Email: [Email]" \
     -H "X-Auth-Key: [API Key]" \
     -H "Content-Type: application/json"
```
You need to replace with [] with your parameter. Then, you get the `id` in `result` which is you **Record ID**.

### Setup DDNS
1. Enter the parameters to the `cloudflareddns.sh`.
2. Login to your DSM
3. Go to Control Panel > External Access > DDNS > Add
4. Select Cloudflare as service provider. Enter your domain as hostname, your Cloudflare account as Username/Email, and API key as Password/Key

## Parameters
### \_\_USERNAME\_\_, \_\_PASSWORD\_\_, \_\_HOSTNAME\_\_, \_\_MYIP\_\_
These are the parameters from Synology.

### \_\_RECTYPE\_\_
DNS record type

### \_\_RECID\_\_
Record ID

### \_\_ZONE\_ID\_\_
Zone ID

### \_\_TTL\_\_
Time to live for DNS record. Value of 1 is 'automatic'

### \_\_PROXY\_\_
Whether the record is receiving the performance and security benefits of Cloudflare.

### \_\_LOGFILE\_\_
Log file location

You can read the [Cloudflare API documentation v4](https://api.cloudflare.com/#dns-records-for-a-zone-update-dns-record) for more details.
