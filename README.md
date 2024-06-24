# better-vllm-api
Openai server for vllm with ability to start and stop the engine, and change the model being served without restarting

# Usage

```
python api_server.py [usual openai api vllm args]
```

# New routes

```python
# loads an engine from the config provided. Config is just the key value pairs you would normally pass in to the command line to load a model
@app.post("/load_engine")
async def load_engine(config: Dict):

# unloads the engine (removes from gpu/ cpu memory)
@app.post("/unload_engine")
async def unload_engine():

# changes the default behaviour if a completions endpoint is called and the model is not loaded
# if autoload is true, then the model will be loaded if needed when a request is made
# if autoload is false, then the request will fail with an http 400 error
@app.post("/toggle_autoload")
async def toggle_autoload(enable: bool):

# gives info about if autoload is on, if the engine is loaded, and what model is loaded
@app.get("/engine_status")
async def engine_status():
    return {
        "engine_loaded": engine is not None,
        "auto_load_enabled": auto_load_enabled,
        "model_name": served_model_names[0] if served_model_names else None,
    }
```
