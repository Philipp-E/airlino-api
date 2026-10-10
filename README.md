# airlino-api

`airlino-api` is an asynchronous Python client for the HTTP API used by LinTech AirLino media players. It supports device and network information, playback, radio streams, volume, and Songcast multiroom operations. It is a third-party community project and is not affiliated with LinTech GmbH.

## Installation

```bash
python -m pip install airlino-api
```

## Quick start

```python
import asyncio

from airlino_api import AirlinoApi


async def main() -> None:
    api = AirlinoApi("192.168.1.100")
    try:
        info = await api.async_get_device_info()
        print(info)
        await api.async_play()
    finally:
        await api.async_close()


asyncio.run(main())
```

The default port is `8989` and the default API version is `v22`. Override them when constructing the client if your device uses different settings:

```python
api = AirlinoApi("airlino.local", port=8989, api_version="v22")
```

## HTTP session lifecycle

The client creates and owns an `aiohttp.ClientSession` when no session is supplied. Close the client with `await api.async_close()` when finished. If you pass an existing session, the caller owns it and must close it; `async_close()` will not close that supplied session.

```python
import aiohttp
from airlino_api import AirlinoApi

async with aiohttp.ClientSession() as session:
    api = AirlinoApi("192.168.1.100", session=session)
    info = await api.async_get_device_info()
```

## Errors

`AirlinoApiConnectionError` indicates a connection or timeout problem. `AirlinoApiError` indicates an HTTP, device, response, or validation error; its `status` attribute contains the HTTP status code when available.

## Home Assistant

This client is used by the [AirLino Home Assistant integration](https://github.com/Philipp-E/home-assistant.io/blob/feature-airlino_integration/source/_integrations/airlino.markdown). It requires Python 3.11 or later.
