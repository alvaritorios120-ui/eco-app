import streamlit as st

# Configuración de la página
st.set_page_config(page_title="EcoCalculator - Álvaro Ríos", layout="centered")

# --- 1. IMAGEN DE INICIO ---
# Reemplaza el link de abajo por el link de tu logo o foto
st.image("IMG-20260418-WA0006.jpg", use_container_width=True)

st.title("🌱 Mi Calculadora Ecológica")
st.markdown("### Gestiona tus materiales y cuida el planeta")

# --- 2. BASE DE DATOS ACTUALIZADA ---
materiales_db = {
    "Botella de plástico (500 ml)": {"co2": 0.2, "co2_rec": 0.05, "costo": 12.0},
    "Bolsa plástica": {"co2": 0.03, "co2_rec": 0.01, "costo": 1.0},
    "Caja de cartón (mediana)": {"co2": 0.7, "co2_rec": 0.2, "costo": 15.0},
    "Periódico": {"co2": 0.2, "co2_rec": 0.05, "costo": 10.0},
    "Lata de soda (aluminio)": {"co2": 0.15, "co2_rec": 0.02, "costo": 20.0},
    "Envase de shampoo": {"co2": 0.3, "co2_rec": 0.08, "costo": 80.0},
    "Lata de comida": {"co2": 0.4, "co2_rec": 0.1, "costo": 45.0},
    "Botella de pegamento": {"co2": 0.25, "co2_rec": 0.07, "costo": 35.0}
}

# --- LÓGICA DEL SISTEMA ---
presupuesto = st.number_input("Presupuesto disponible (C$):", min_value=0.0, value=500.0)

st.subheader("Selecciona tus materiales")
seleccionados = st.multiselect("Añadir productos:", list(materiales_db.keys()))

total_co2 = 0
total_gasto = 0

if seleccionados:
    for item in seleccionados:
        with st.expander(f"Detalles de: {item}"):
            c1, c2 = st.columns(2)
            with c1:
                cantidad = st.number_input(f"Cantidad de {item}:", min_value=1, value=1, key=f"c_{item}")
            with c2:
                reciclados = st.number_input(f"¿Cuántos reciclaste?:", min_value=0, max_value=cantidad, value=0, key=f"r_{item}")
            
            # Cálculos
            datos = materiales_db[item]
            total_gasto += cantidad * datos["costo"]
            total_co2 += ((cantidad - reciclados) * datos["co2"]) + (reciclados * datos["co2_rec"])

    # Resultados
    st.divider()
    res1, res2 = st.columns(2)
    res1.metric("Impacto CO2 Total", f"{round(total_co2, 2)} kg")
    res2.metric("Gasto Total", f"C$ {total_gasto}")

    if total_co2 < 0.5:
        st.success("✅ ¡Impacto muy bajo! Estás ayudando al medio ambiente.")
    elif total_co2 < 1.5:
        st.warning("⚠️ Impacto moderado. Intenta reciclar más piezas.")
    else:
        st.error("🚨 Impacto alto. Considera usar materiales menos contaminantes.")
else:
    st.info("Elige materiales arriba para ver el impacto.")
