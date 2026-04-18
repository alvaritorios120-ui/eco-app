import streamlit as st

# Configuración de la página
st.set_page_config(page_title="EcoCalculator - Sistema de Impacto Ambiental", layout="centered")

# --- BASE DE DATOS DE MATERIALES ---
materiales_db = {
    "Botella de plástico (500 ml)": {"co2": 0.2, "co2_rec": 0.05, "costo": 2.0},
    "Bolsa plástica": {"co2": 0.03, "co2_rec": 0.01, "costo": 0.5},
    "Caja de cartón (mediana)": {"co2": 0.7, "co2_rec": 0.2, "costo": 5.0},
    "Periódico": {"co2": 0.2, "co2_rec": 0.05, "costo": 1.0},
    "Lata de soda (aluminio)": {"co2": 0.15, "co2_rec": 0.02, "costo": 3.0},
    "Envase de shampoo": {"co2": 0.3, "co2_rec": 0.08, "costo": 10.0},
    "Lata de comida": {"co2": 0.4, "co2_rec": 0.1, "costo": 8.0},
    "Botella de pegamento": {"co2": 0.25, "co2_rec": 0.07, "costo": 4.0}
}

# --- ENCABEZADO Y LOGOTIPO ---
# Nota: Para el logo, puedes usar una URL de una imagen o subirla a tu repositorio
st.image("https://via.placeholder.com/150x150.png?text=TU+LOGO", width=120) 
st.title("🌱 Sistema de Gestión de Impacto Ambiental")
st.markdown("Calcula tu huella de carbono y gestiona tu presupuesto de materiales.")

# --- ENTRADA DE PRESUPUESTO ---
presupuesto = st.number_input("Ingresa tu presupuesto total (C$):", min_value=0.0, value=100.0)

# --- SELECCIÓN DINÁMICA DE PRODUCTOS ---
st.subheader("Selección de Materiales")
lista_seleccion = st.multiselect("¿Qué materiales vas a usar hoy?", list(materiales_db.keys()))

totales_co2 = 0
totales_gasto = 0

if lista_seleccion:
    for item in lista_seleccion:
        with st.expander(f"Configurar: {item}"):
            col1, col2 = st.columns(2)
            with col1:
                cant = st.number_input(f"Cantidad de {item}:", min_value=1, value=1, key=f"cant_{item}")
            with col2:
                reciclado = st.number_input(f"¿Cuántos reciclaste de esos {cant}?:", 
                                            min_value=0, max_value=cant, value=0, key=f"rec_{item}")
            
            # Cálculos por item
            datos = materiales_db[item]
            gasto_item = cant * datos["costo"]
            # CO2: (Lo que no reciclaste * co2 normal) + (Lo que reciclaste * co2 reciclado)
            co2_item = ((cant - reciclado) * datos["co2"]) + (reciclado * datos["co2_rec"])
            
            totales_co2 += co2_item
            totales_gasto += gasto_item
            
    # --- RESULTADOS FINALES ---
    st.divider()
    st.header("📊 Resultados del Sistema")
    
    c1, c2, c3 = st.columns(3)
    c1.metric("CO2 Total (kg)", f"{round(totales_co2, 2)} kg")
    c2.metric("Gasto Total", f"C${totales_gasto}")
    c3.metric("Saldo", f"C${round(presupuesto - totales_gasto, 2)}")

    # Clasificación automática
    if totales_co2 < 0.5:
        st.success("✅ Impacto Ambiental: BAJO. ¡Excelente elección!")
    elif totales_co2 < 2.0:
        st.warning("⚠️ Impacto Ambiental: MODERADO. Considera reciclar más.")
    else:
        st.error("🚨 Impacto Ambiental: ALTO. Tu huella de carbono es considerable.")
else:
    st.info("Selecciona materiales arriba para comenzar el cálculo.")
