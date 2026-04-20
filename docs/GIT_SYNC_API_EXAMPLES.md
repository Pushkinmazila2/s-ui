# Git Sync API Examples

## Configuration Examples

### GitHub Configuration

```bash
curl -X POST http://localhost:2095/app/api/gitSyncConfig \
  -H "Content-Type: application/json" \
  -H "Cookie: session=your_session_cookie" \
  -d '{
    "enable": true,
    "provider": "github",
    "repoUrl": "https://github.com/username/s-ui-backup",
    "branch": "main",
    "token": "ghp_xxxxxxxxxxxxxxxxxxxx",
    "autoSync": true,
    "syncInterval": 3600,
    "syncConfig": true,
    "syncDb": true
  }'
```

### GitLab Configuration

```bash
curl -X POST http://localhost:2095/app/api/gitSyncConfig \
  -H "Content-Type: application/json" \
  -H "Cookie: session=your_session_cookie" \
  -d '{
    "enable": true,
    "provider": "gitlab",
    "repoUrl": "https://gitlab.com/username/s-ui-backup",
    "branch": "main",
    "token": "glpat-xxxxxxxxxxxxxxxxxxxx",
    "autoSync": true,
    "syncInterval": 3600,
    "syncConfig": true,
    "syncDb": true
  }'
```

### Gitea Configuration

```bash
curl -X POST http://localhost:2095/app/api/gitSyncConfig \
  -H "Content-Type: application/json" \
  -H "Cookie: session=your_session_cookie" \
  -d '{
    "enable": true,
    "provider": "gitea",
    "repoUrl": "https://gitea.example.com/username/s-ui-backup",
    "branch": "main",
    "token": "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
    "autoSync": true,
    "syncInterval": 3600,
    "syncConfig": true,
    "syncDb": true
  }'
```

## Get Current Configuration

```bash
curl -X GET http://localhost:2095/app/api/gitSyncConfig \
  -H "Cookie: session=your_session_cookie"
```

Response:
```json
{
  "success": true,
  "obj": {
    "id": 1,
    "enable": true,
    "provider": "github",
    "repoUrl": "https://github.com/username/s-ui-backup",
    "branch": "main",
    "token": "***",
    "autoSync": true,
    "syncInterval": 3600,
    "syncConfig": true,
    "syncDb": true,
    "lastSync": 1713643744
  }
}
```

## Manual Push to Git

```bash
curl -X POST http://localhost:2095/app/api/gitSyncPush \
  -H "Cookie: session=your_session_cookie"
```

Response:
```json
{
  "success": true,
  "msg": ""
}
```

## Manual Pull from Git

```bash
curl -X POST http://localhost:2095/app/api/gitSyncPull \
  -H "Cookie: session=your_session_cookie"
```

Response:
```json
{
  "success": true,
  "msg": ""
}
```

## Test Connection

```bash
curl -X POST http://localhost:2095/app/api/gitSyncTest \
  -H "Cookie: session=your_session_cookie"
```

Success Response:
```json
{
  "success": true,
  "msg": ""
}
```

Error Response:
```json
{
  "success": false,
  "msg": "GitHub API error: 401 - Bad credentials"
}
```

## Disable Sync

```bash
curl -X POST http://localhost:2095/app/api/gitSyncConfig \
  -H "Content-Type: application/json" \
  -H "Cookie: session=your_session_cookie" \
  -d '{
    "enable": false,
    "provider": "github",
    "repoUrl": "https://github.com/username/s-ui-backup",
    "branch": "main",
    "token": "ghp_xxxxxxxxxxxxxxxxxxxx",
    "autoSync": false,
    "syncInterval": 3600,
    "syncConfig": true,
    "syncDb": true
  }'
```

## JavaScript/Fetch Examples

### Configure Git Sync

```javascript
fetch('/app/api/gitSyncConfig', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json'
  },
  body: JSON.stringify({
    enable: true,
    provider: 'github',
    repoUrl: 'https://github.com/username/s-ui-backup',
    branch: 'main',
    token: 'ghp_xxxxxxxxxxxxxxxxxxxx',
    autoSync: true,
    syncInterval: 3600,
    syncConfig: true,
    syncDb: true
  })
})
.then(response => response.json())
.then(data => console.log(data));
```

### Push to Git

```javascript
fetch('/app/api/gitSyncPush', {
  method: 'POST'
})
.then(response => response.json())
.then(data => {
  if (data.success) {
    console.log('Successfully pushed to Git');
  } else {
    console.error('Push failed:', data.msg);
  }
});
```

### Test Connection

```javascript
fetch('/app/api/gitSyncTest', {
  method: 'POST'
})
.then(response => response.json())
.then(data => {
  if (data.success) {
    console.log('Connection successful');
  } else {
    console.error('Connection failed:', data.msg);
  }
});
```

## Python Examples

### Configure Git Sync

```python
import requests

url = 'http://localhost:2095/app/api/gitSyncConfig'
headers = {'Content-Type': 'application/json'}
cookies = {'session': 'your_session_cookie'}

data = {
    'enable': True,
    'provider': 'github',
    'repoUrl': 'https://github.com/username/s-ui-backup',
    'branch': 'main',
    'token': 'ghp_xxxxxxxxxxxxxxxxxxxx',
    'autoSync': True,
    'syncInterval': 3600,
    'syncConfig': True,
    'syncDb': True
}

response = requests.post(url, json=data, headers=headers, cookies=cookies)
print(response.json())
```

### Push to Git

```python
import requests

url = 'http://localhost:2095/app/api/gitSyncPush'
cookies = {'session': 'your_session_cookie'}

response = requests.post(url, cookies=cookies)
result = response.json()

if result['success']:
    print('Successfully pushed to Git')
else:
    print(f'Push failed: {result["msg"]}')
```

## Notes

- Replace `localhost:2095` with your actual S-UI server address
- Replace `your_session_cookie` with your actual session cookie
- Replace tokens with your actual access tokens
- All POST requests require authentication via session cookie
- Tokens are masked (shown as `***`) in GET responses for security
