# chatbot-ubuntu
import time
from selenium import webdriver
from selenium.webdriver.chrome.service import Service
from selenium.webdriver.chrome.options import Options
from selenium.webdriver.common.by import By
from webdriver_manager.chrome import ChromeDriverManager

def iniciar_driver():
    chrome_options = Options()
    # Para visualizar en Ubuntu, dejamos que la ventana se abra
    chrome_options.add_argument("--start-maximized")
    chrome_options.add_argument("--no-sandbox")
    chrome_options.add_argument("--disable-dev-shm-usage")
    
    service = Service(ChromeDriverManager().install())
    return webdriver.Chrome(service=service, options=chrome_options)

def consultar_google(driver, busqueda, selector):
    try:
        driver.get(f"https://www.google.com/search?q={busqueda}")
        time.sleep(2)
        dato = driver.find_element(By.CSS_SELECTOR, selector).text
        return dato
    except:
        return "No encontrado"

if __name__ == "__main__":
    driver = iniciar_driver()
    print("\n--- BOT MULTIFUNCIÓN (ACCIONES Y CLIMA) ---")
    
    try:
        while True:
            print("\n1. Precio Acción | 2. Temperatura | 3. Salir")
            opc = input("Selecciona una opción: ")
            
            if opc == "1":
                empresa = input("Empresa (ej. Tesla): ")
                # Selector para precio de acción en Google
                precio = consultar_google(driver, f"precio+accion+{empresa}", "span[jsname='vWLAgc']")
                print(f"Chatbot: El precio de {empresa} es {precio}")
            
            elif opc == "2":
                ciudad = input("Ciudad (ej. Madrid): ")
                # Selector para temperatura en Google
                temp = consultar_google(driver, f"temperatura+en+{ciudad}", "span#wob_tm")
                print(f"Chatbot: La temperatura en {ciudad} es {temp}°C")
            
            elif opc == "3":
                break
    finally:
        driver.quit()
