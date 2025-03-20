from fastapi import FastAPI, Form
from fastapi.responses import HTMLResponse
from fastapi.staticfiles import StaticFiles
from fastapi.templating import Jinja2Templates
from starlette.requests import Request

app = FastAPI()

# Configurar rutas de archivos estáticos y plantillas
app.mount("/static", StaticFiles(directory="static"), name="static")
templates = Jinja2Templates(directory="templates")

@app.get("/", response_class=HTMLResponse)
async def home(request: Request):
    return templates.TemplateResponse("index.html", {"request": request})

@app.get("/objetivos", response_class=HTMLResponse)
async def objetivos(request: Request):
    return templates.TemplateResponse("objetivos.html", {"request": request})

@app.get("/perfiles", response_class=HTMLResponse)
async def perfiles(request: Request):
    return templates.TemplateResponse("perfiles.html", {"request": request})

@app.get("/opinion", response_class=HTMLResponse)
async def opinion(request: Request):
    return templates.TemplateResponse("opinion.html", {"request": request})

@app.post("/opinion")
async def recibir_opinion(name: str = Form(...), email: str = Form(...), opinion: str = Form(...)):
    print(f"Nombre: {name}, Email: {email}, Opinión: {opinion}")
    return {"message": "¡Gracias por tu opinión!"}

if __name__ == "__main__":
    import uvicorn
    uvicorn.run(app, host="0.0.0.0", port=8000, reload=True)
