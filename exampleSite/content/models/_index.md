---
title: Models
next: docs/pca
---

This is where I will put my models and results.

## Introduction

```go {filename="main.go"}
package main

import "fmt"

func main() {
    fmt.Println("Hello, World!")
}
```

```python {filename="ml.py"}
print('Hello world!!')
```

> [!TIP]
> Helpful advice for doing things better or more easily.

> [!IMPORTANT]
> Key information users need to know to achieve their goal.

> [!WARNING]
> Urgent info that needs immediate user attention to avoid problems.

{{< tabs items="Scikit-learn,My implementation" >}}

  {{< tab >}}
  ```python
  print('Scikit-learn')
  ```
  {{< /tab >}}
  {{< tab >}}
  ```python
  print('My implementation')
  ```
  {{< /tab >}}
{{< /tabs >}}

{{% details title="Spotify API Code" %}}

{{< filetree/container >}}
  {{< filetree/folder name="csci5612-workspace" state="open">}}
    {{< filetree/folder name="src" state="open">}}
      {{< filetree/folder name="api" state="open">}}
        {{< filetree/folder name="spotify" state="open">}}
          {{< filetree/file name="authenticator.py" >}}
        {{< /filetree/folder >}}
      {{< /filetree/folder >}}
    {{< /filetree/folder >}}
  {{< /filetree/folder >}}
{{< /filetree/container >}}

See [authenticator.py](#).

{{< tabs items="Authenticator,Searcher" >}}

  {{< tab >}}
  ```python {filename="authenticator.py"}
  import os
  import time
  import json
  import base64
  import requests
  import subprocess
  import sys

  subprocess.check_call([sys.executable, '-m', 'pip', 'install', 'python-dotenv'])
  from dotenv import load_dotenv

  from utils.logger import logger

  load_dotenv()

  TOKEN_ENDPOINT = 'https://accounts.spotify.com/api/token'
  TOKEN_FILE = 'spotify_token.json'

  def get_spotify_credentials():
      """Load Spotify API credentials from .env file."""
      client_id = os.getenv('CLIENT_ID')
      client_secret = os.getenv('CLIENT_SECRET')
      
      if not client_id or not client_secret:
          raise ValueError('Missing Spotify credentials. Need CLIENT_ID and CLIENT_SECRET')
          
      return client_id, client_secret

  def load_token():
      """Loads most recent access token."""
      if os.path.exists(TOKEN_FILE):
          try:
              with open(TOKEN_FILE, 'r') as file:
                  data = json.load(file)
                  return data.get('access_token'), data.get('expires_at', 0)
          except (json.JSONDecodeError, FileNotFoundError) as e:
              logger.warning(f'Could not load token file: {e}')
              
      return None, 0

  def save_token(access_token, expires_at):
      """Saves access token for persistence."""
      with open(TOKEN_FILE, 'w') as file:
          json.dump({'access_token': access_token, 'expires_at': expires_at}, file)
          
  def get_token():
      """Returns access token or refreshes token if expired."""
      access_token, expires_at = load_token()
      
      if access_token and time.time() < expires_at:
          return access_token
      
      logger.info('Refreshing Spotify access token...')
      
      client_id, client_secret = get_spotify_credentials()
      credentials = f'{client_id}:{client_secret}'
      encoded_credentials = base64.b64encode(credentials.encode()).decode()
      
      headers = {
          'Authorization': f'Basic {encoded_credentials}',
          'Content-Type': 'application/x-www-form-urlencoded'
      }
      data = {'grant_type': 'client_credentials'}
      
      response = requests.post(TOKEN_ENDPOINT, headers=headers, data=data)
      
      if response.status_code == 200:
          token_data = response.json()
          access_token = token_data['access_token']
          expires_at = time.time() + token_data['expires_in'] - 60
          
          save_token(access_token, expires_at)
          logger.info('Access token refreshed successfully!')
          return access_token
      else:
          message = f'Failed to refresh token: {response.status_code}, {response.text}'
          logger.error(message)
          raise Exception(message)
  ```
  {{< /tab >}}
  {{< tab >}}
  ```python
  print('My implementation')
  ```
  {{< /tab >}}
{{< /tabs >}}

{{% /details %}}

## End