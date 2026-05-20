# app.py
# Ejecuta:
# pip install flask
# python app.py

from flask import Flask, render_template_string, request, jsonify
from datetime import datetime

app = Flask(__name__)

QUESTIONS = [
    {
        "id": 1,
        "topic": "Naturaleza de los fluidos",
        "level": "Conceptual avanzado",
        "question": "Desde la perspectiva mecánica, ¿qué criterio permite distinguir de manera rigurosa a un fluido de un sólido elástico cuando ambos son sometidos a esfuerzo cortante?",
        "options": [
            "El fluido puede soportar esfuerzos normales, pero no esfuerzos tangenciales.",
            "El fluido se deforma continuamente bajo cualquier esfuerzo cortante, mientras el sólido puede alcanzar equilibrio deformado.",
            "El sólido cambia de volumen ante presión, mientras el fluido conserva siempre volumen constante.",
            "El fluido solo se deforma cuando el esfuerzo cortante supera su módulo volumétrico."
        ],
        "answer": 1,
        "explanation": "Un fluido no puede permanecer en equilibrio estático bajo esfuerzo cortante; se deforma continuamente, aun cuando el esfuerzo sea pequeño."
    },
    {
        "id": 2,
        "topic": "Líquidos y gases",
        "level": "Conceptual avanzado",
        "question": "Un gas confinado en un recipiente con pistón móvil reduce notablemente su volumen al aumentar la presión. ¿Qué conclusión técnica es correcta?",
        "options": [
            "Los gases son prácticamente incompresibles porque llenan todo el recipiente.",
            "Los gases son fácilmente compresibles, a diferencia de los líquidos que cambian muy poco su volumen.",
            "La compresibilidad de gases y líquidos es equivalente si ambos están a presión atmosférica.",
            "El gas reduce su volumen únicamente porque disminuye su masa."
        ],
        "answer": 1,
        "explanation": "Los gases son fácilmente compresibles; los líquidos solo son ligeramente compresibles."
    },
    {
        "id": 3,
        "topic": "Presión",
        "level": "Cálculo aplicado",
        "question": "Un pistón aplica una fuerza perpendicular de 12.0 kN sobre aceite. Si el diámetro del pistón es 75 mm, ¿cuál es la presión aproximada en el aceite?",
        "options": [
            "2.72 MPa",
            "0.213 MPa",
            "27.2 MPa",
            "21.3 kPa"
        ],
        "answer": 0,
        "explanation": "A = πD²/4 = π(0.075)²/4 = 0.004418 m². p = F/A = 12000/0.004418 ≈ 2.72 MPa."
    },
    {
        "id": 4,
        "topic": "Leyes de Pascal",
        "level": "Conceptual avanzado",
        "question": "¿Cuál afirmación describe correctamente el comportamiento de la presión en un fluido confinado por fronteras sólidas?",
        "options": [
            "La presión actúa únicamente en la dirección de la fuerza aplicada originalmente.",
            "La presión actúa paralela a las paredes si el fluido está en movimiento.",
            "La presión actúa perpendicularmente a las fronteras sólidas y se transmite en todas las direcciones.",
            "La presión disminuye automáticamente en las paredes laterales por efecto de la viscosidad."
        ],
        "answer": 2,
        "explanation": "Según los principios de Pascal, la presión actúa uniformemente en todas las direcciones y perpendicular a las fronteras."
    },
    {
        "id": 5,
        "topic": "Sistemas de unidades",
        "level": "Análisis dimensional",
        "question": "En el SI, ¿por qué el newton puede expresarse como kg·m/s²?",
        "options": [
            "Porque la fuerza es una magnitud básica independiente de la masa.",
            "Porque se deriva de F = ma, donde la masa está en kg y la aceleración en m/s².",
            "Porque el peso específico se define como fuerza por unidad de volumen.",
            "Porque el pascal es equivalente a N/m²."
        ],
        "answer": 1,
        "explanation": "La unidad de fuerza se obtiene de F = ma; por ello, 1 N = 1 kg·m/s²."
    },
    {
        "id": 6,
        "topic": "Peso y masa",
        "level": "Conceptual difícil",
        "question": "¿Cuál es la diferencia técnica más precisa entre masa y peso?",
        "options": [
            "La masa es una fuerza y el peso es una propiedad escalar independiente de la gravedad.",
            "La masa mide la inercia o cantidad de materia; el peso es la fuerza gravitatoria ejercida sobre esa masa.",
            "La masa solo existe en el SI y el peso solo existe en el sistema inglés.",
            "El peso permanece constante en cualquier planeta, mientras la masa cambia con la gravedad."
        ],
        "answer": 1,
        "explanation": "La masa mide la inercia o cantidad de sustancia; el peso depende de la gravedad: w = mg."
    },
    {
        "id": 7,
        "topic": "Peso y masa",
        "level": "Cálculo aplicado",
        "question": "Un componente tiene masa de 150 kg en un lugar donde g = 9.6 m/s². ¿Cuál es su peso?",
        "options": [
            "15.625 N",
            "1440 N",
            "1562.5 N",
            "14.4 kN"
        ],
        "answer": 1,
        "explanation": "w = mg = 150 × 9.6 = 1440 N."
    },
    {
        "id": 8,
        "topic": "Sistema inglés",
        "level": "Conceptual difícil",
        "question": "En el sistema inglés gravitacional coherente, ¿cuál es la unidad derivada de masa?",
        "options": [
            "lbf",
            "lbm",
            "slug",
            "psi"
        ],
        "answer": 2,
        "explanation": "En el sistema inglés gravitacional, la unidad derivada coherente de masa es el slug."
    },
    {
        "id": 9,
        "topic": "lbm y lbf",
        "level": "Conceptual difícil",
        "question": "¿Por qué la equivalencia numérica entre lbm y lbf solo es válida bajo gravedad estándar?",
        "options": [
            "Porque lbm y lbf son siempre la misma unidad física.",
            "Porque la relación peso-masa en lbm requiere la constante gc y depende del valor local de g.",
            "Porque la masa cambia con la aceleración de la gravedad.",
            "Porque la libra-fuerza solo se usa para fluidos líquidos."
        ],
        "answer": 1,
        "explanation": "Con lbm se usa F = m(a/gc). Si g cambia, el peso en lbf ya no coincide numéricamente con la masa en lbm."
    },
    {
        "id": 10,
        "topic": "lbm y lbf",
        "level": "Caso aplicado difícil",
        "question": "Un astronauta de 195 lbm está en la Luna, donde g = 5.48 ft/s². ¿Qué leería aproximadamente una báscula de resorte?",
        "options": [
            "195 lbf",
            "33.2 lbf",
            "5.48 lbf",
            "1176 lbf"
        ],
        "answer": 1,
        "explanation": "w = m(g/gc) = 195(5.48/32.174) ≈ 33.2 lbf."
    },
    {
        "id": 11,
        "topic": "Volumen de control",
        "level": "Conceptual avanzado",
        "question": "Al analizar el flujo de gas a través de una boquilla, ¿qué tipo de sistema resulta más adecuado?",
        "options": [
            "Sistema cerrado, porque la masa dentro de la boquilla permanece fija.",
            "Volumen de control, porque la masa cruza las fronteras de entrada y salida.",
            "Sistema aislado, porque no hay transferencia de energía.",
            "Sistema rígido, porque el fluido no cambia de velocidad."
        ],
        "answer": 1,
        "explanation": "Una boquilla se analiza como volumen de control porque hay flujo de masa a través de sus fronteras."
    },
    {
        "id": 12,
        "topic": "Sistema cerrado y volumen de control",
        "level": "Conceptual difícil",
        "question": "¿Qué criterio diferencia esencialmente a un sistema cerrado de un volumen de control?",
        "options": [
            "La transferencia de calor a través de la frontera.",
            "La posibilidad de que la masa cruce la frontera.",
            "La existencia de presión atmosférica.",
            "La presencia de trabajo de eje."
        ],
        "answer": 1,
        "explanation": "En un sistema cerrado no cruza masa; en un volumen de control sí puede entrar y salir masa."
    },
    {
        "id": 13,
        "topic": "Flujo estacionario",
        "level": "Conceptual difícil",
        "question": "¿Cuál definición corresponde a un proceso de flujo estacionario?",
        "options": [
            "La masa total del sistema aumenta con el tiempo de forma constante.",
            "Las propiedades en un punto espacial no cambian respecto al tiempo.",
            "La velocidad del fluido es cero en todo el dominio.",
            "La presión disminuye linealmente en la dirección del flujo."
        ],
        "answer": 1,
        "explanation": "En flujo estacionario, las propiedades en cada punto no varían con el tiempo."
    },
    {
        "id": 14,
        "topic": "Flujo incompresible",
        "level": "Criterio técnico",
        "question": "En un sistema de ventilación con aire a Ma = 0.12, ¿por qué puede aplicarse el modelo de flujo incompresible?",
        "options": [
            "Porque todo flujo de aire es incompresible a presión atmosférica.",
            "Porque Ma < 0.3 implica variaciones de densidad pequeñas, usualmente menores al 5%.",
            "Porque la viscosidad del aire desaparece a bajo Mach.",
            "Porque el régimen laminar garantiza densidad constante."
        ],
        "answer": 1,
        "explanation": "Para Ma < 0.3, los cambios de densidad suelen ser despreciables para análisis incompresible."
    },
    {
        "id": 15,
        "topic": "Número de Mach",
        "level": "Conceptual aplicado",
        "question": "Si un flujo de vapor en una tubería tiene Ma = 2, ¿qué interpretación física es correcta?",
        "options": [
            "La presión del vapor es el doble de la presión atmosférica.",
            "La velocidad del fluido es el doble de la velocidad local del sonido.",
            "La densidad del fluido es dos veces la densidad crítica.",
            "La viscosidad cinemática se duplica por compresibilidad."
        ],
        "answer": 1,
        "explanation": "Ma = V/c. Si Ma = 2, la velocidad del flujo es dos veces la velocidad local del sonido."
    },
    {
        "id": 16,
        "topic": "Capa límite",
        "level": "Conceptual avanzado",
        "question": "¿Cuál es la causa física principal del desarrollo de la capa límite sobre una superficie sólida?",
        "options": [
            "La presión hidrostática uniforme en la pared.",
            "La condición de no deslizamiento generada por la viscosidad.",
            "La compresibilidad del fluido a cualquier velocidad.",
            "La tensión superficial entre líquido y gas."
        ],
        "answer": 1,
        "explanation": "La viscosidad impone no deslizamiento en la pared, creando fuertes gradientes de velocidad y crecimiento de capa límite."
    },
    {
        "id": 17,
        "topic": "Esfuerzo cortante y presión",
        "level": "Conceptual difícil",
        "question": "¿Cuál afirmación define correctamente esfuerzo cortante y presión hidrostática?",
        "options": [
            "El esfuerzo cortante es normal al área y la presión es tangencial a la pared.",
            "El esfuerzo cortante es fuerza tangencial por unidad de área; la presión es esfuerzo normal en un fluido en reposo.",
            "Ambos son propiedades termodinámicas independientes del área.",
            "La presión solo existe en fluidos viscosos y el esfuerzo cortante solo en gases."
        ],
        "answer": 1,
        "explanation": "El esfuerzo cortante es tangencial; la presión hidrostática es normal a la superficie."
    },
    {
        "id": 18,
        "topic": "Consistencia dimensional",
        "level": "Análisis dimensional",
        "question": "Se obtiene la ecuación E = 16 kJ + 7 kJ/kg. ¿Cuál es el error dimensional?",
        "options": [
            "No hay error, porque ambos términos contienen kJ.",
            "El primer término debe dividirse entre la masa para obtener kJ/kg.",
            "No pueden sumarse energía total y energía específica; el segundo término debe multiplicarse por masa si se desea energía total.",
            "El segundo término debe convertirse a kN para que sea compatible."
        ],
        "answer": 2,
        "explanation": "kJ y kJ/kg no son unidades homogéneas. Para sumar energía total, 7 kJ/kg debe multiplicarse por una masa."
    },
    {
        "id": 19,
        "topic": "Densidad",
        "level": "Conceptual",
        "question": "¿Cuál es la definición técnica de densidad?",
        "options": [
            "Peso por unidad de volumen.",
            "Masa por unidad de volumen.",
            "Fuerza por unidad de área.",
            "Peso relativo respecto al aire."
        ],
        "answer": 1,
        "explanation": "La densidad se define como ρ = m/V."
    },
    {
        "id": 20,
        "topic": "Peso específico",
        "level": "Conceptual",
        "question": "¿Cuál es la definición técnica de peso específico?",
        "options": [
            "Masa por unidad de volumen.",
            "Peso por unidad de volumen.",
            "Presión por unidad de profundidad.",
            "Densidad dividida entre gravedad."
        ],
        "answer": 1,
        "explanation": "El peso específico se define como γ = w/V."
    },
    {
        "id": 21,
        "topic": "Densidad y peso específico",
        "level": "Cálculo aplicado",
        "question": "La glicerina tiene gravedad específica sg = 1.263. Usando ρagua = 1000 kg/m³, ¿cuál es su densidad?",
        "options": [
            "126.3 kg/m³",
            "1263 kg/m³",
            "12.63 kg/m³",
            "0.001263 kg/m³"
        ],
        "answer": 1,
        "explanation": "ρ = sg × ρagua = 1.263 × 1000 = 1263 kg/m³."
    },
    {
        "id": 22,
        "topic": "Densidad y peso específico",
        "level": "Cálculo aplicado",
        "question": "Un aceite tiene masa de 825 kg y volumen de 0.917 m³. ¿Cuál es su densidad aproximada?",
        "options": [
            "900 kg/m³",
            "0.00111 kg/m³",
            "8.83 kN/m³",
            "917 kg/m³"
        ],
        "answer": 0,
        "explanation": "ρ = m/V = 825/0.917 ≈ 900 kg/m³."
    },
    {
        "id": 23,
        "topic": "Gravedad específica",
        "level": "Conceptual avanzado",
        "question": "La gravedad específica de una sustancia se define como:",
        "options": [
            "La relación entre su presión y la presión atmosférica.",
            "La relación entre su densidad o peso específico y los valores correspondientes del agua a 4 °C.",
            "La relación entre su masa y su peso.",
            "La relación entre su volumen inicial y final al comprimirse."
        ],
        "answer": 1,
        "explanation": "sg = ρs/ρagua a 4 °C = γs/γagua a 4 °C."
    },
    {
        "id": 24,
        "topic": "Compresibilidad",
        "level": "Conceptual avanzado",
        "question": "¿Qué representa el módulo volumétrico de elasticidad de un líquido?",
        "options": [
            "La relación entre esfuerzo cortante y deformación angular.",
            "La resistencia del fluido a cambiar su volumen ante cambios de presión.",
            "La facilidad con la que un fluido fluye por una tubería.",
            "La relación entre tensión superficial y radio capilar."
        ],
        "answer": 1,
        "explanation": "El módulo volumétrico mide la presión requerida para producir un cambio relativo de volumen."
    },
    {
        "id": 25,
        "topic": "Compresibilidad",
        "level": "Cálculo difícil",
        "question": "El agua tiene módulo volumétrico aproximado E = 316000 psi. ¿Qué cambio de presión se requiere para reducir su volumen en 1.0%?",
        "options": [
            "316 psi",
            "3160 psi",
            "31600 psi",
            "31.6 psi"
        ],
        "answer": 1,
        "explanation": "Δp = -E(ΔV/V). Para ΔV/V = -0.01: Δp = 316000 × 0.01 = 3160 psi."
    },
    {
        "id": 26,
        "topic": "Tensión superficial",
        "level": "Conceptual avanzado",
        "question": "¿Cuál afirmación explica mejor por qué una aguja pequeña puede apoyarse sobre agua quieta y hundirse al agregar detergente?",
        "options": [
            "El detergente aumenta la densidad del agua hasta superar la del metal.",
            "El detergente reduce la tensión superficial que sostenía parcialmente la aguja.",
            "El detergente transforma el agua en un fluido no viscoso.",
            "La aguja flota por empuje hidrostático y el detergente elimina la gravedad."
        ],
        "answer": 1,
        "explanation": "El detergente reduce la tensión superficial, por lo que la superficie ya no puede sostener la aguja."
    },
    {
        "id": 27,
        "topic": "Tensión superficial",
        "level": "Unidades",
        "question": "¿Cuáles son unidades coherentes para la tensión superficial?",
        "options": [
            "N/m o lb/ft",
            "N/m² o psi",
            "kg/m³ o slugs/ft³",
            "N·m o ft·lb"
        ],
        "answer": 0,
        "explanation": "La tensión superficial se expresa como fuerza por unidad de longitud: N/m o lb/ft."
    },
    {
        "id": 28,
        "topic": "Presión hidráulica",
        "level": "Cálculo difícil",
        "question": "Un cilindro hidráulico debe ejercer 38.8 kN con un pistón de 40 mm de diámetro. ¿Qué presión aproximada requiere el aceite?",
        "options": [
            "3.09 MPa",
            "30.9 MPa",
            "309 kPa",
            "0.0309 MPa"
        ],
        "answer": 1,
        "explanation": "A = π(0.04)²/4 = 0.001257 m². p = 38800/0.001257 ≈ 30.9 MPa."
    },
    {
        "id": 29,
        "topic": "Flujo interno y canal abierto",
        "level": "Conceptual difícil",
        "question": "¿Qué criterio técnico transforma el análisis de una conducción circular desde flujo interno presurizado hacia flujo en canal abierto?",
        "options": [
            "La tubería cambia de material metálico a plástico.",
            "Aparece una superficie libre donde la presión coincide con la presión atmosférica local.",
            "El número de Reynolds supera el valor crítico.",
            "El fluido cambia de líquido a gas."
        ],
        "answer": 1,
        "explanation": "El flujo en canal abierto se caracteriza por la existencia de superficie libre sometida a presión atmosférica."
    },
    {
        "id": 30,
        "topic": "Clasificación de flujos",
        "level": "Aplicación conceptual",
        "question": "En una aeronave, el flujo sobre el extradós del ala y el flujo dentro de una turbina se clasifican respectivamente como:",
        "options": [
            "Interno e interno.",
            "Externo e interno.",
            "Interno y externo.",
            "Canal abierto y flujo libre."
        ],
        "answer": 1,
        "explanation": "Sobre el ala el flujo no está confinado: externo. Dentro de la turbina está limitado por fronteras sólidas: interno."
    }
]

HTML = """
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Examen Dinámico | Mecánica de Fluidos</title>
    <style>
        :root {
            --bg: #07111f;
            --panel: rgba(255,255,255,.08);
            --panel-2: rgba(255,255,255,.13);
            --text: #f4f7fb;
            --muted: #a8b3c7;
            --accent: #38bdf8;
            --accent-2: #22c55e;
            --danger: #ef4444;
            --warning: #f59e0b;
            --purple: #a78bfa;
            --border: rgba(255,255,255,.15);
            --shadow: 0 25px 80px rgba(0,0,0,.35);
        }

        * {
            box-sizing: border-box;
        }

        body {
            margin: 0;
            font-family: Inter, system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif;
            background:
                radial-gradient(circle at top left, rgba(56,189,248,.26), transparent 35%),
                radial-gradient(circle at bottom right, rgba(167,139,250,.22), transparent 35%),
                linear-gradient(135deg, #06101d 0%, #0f172a 55%, #111827 100%);
            color: var(--text);
            min-height: 100vh;
            overflow-x: hidden;
        }

        .app {
            width: min(1180px, calc(100% - 28px));
            margin: 0 auto;
            padding: 28px 0 40px;
        }

        .hero {
            display: grid;
            grid-template-columns: 1.3fr .7fr;
            gap: 20px;
            align-items: stretch;
            margin-bottom: 20px;
        }

        .glass {
            background: var(--panel);
            border: 1px solid var(--border);
            border-radius: 28px;
            box-shadow: var(--shadow);
            backdrop-filter: blur(18px);
        }

        .intro {
            padding: 28px;
            position: relative;
            overflow: hidden;
        }

        .intro::after {
            content: "";
            position: absolute;
            width: 220px;
            height: 220px;
            border-radius: 999px;
            background: rgba(56,189,248,.15);
            top: -70px;
            right: -50px;
            filter: blur(2px);
        }

        .badge {
            display: inline-flex;
            align-items: center;
            gap: 8px;
            padding: 8px 12px;
            border: 1px solid var(--border);
            border-radius: 999px;
            background: rgba(255,255,255,.08);
            color: #dbeafe;
            font-weight: 700;
            font-size: 13px;
            letter-spacing: .2px;
        }

        h1 {
            margin: 16px 0 10px;
            font-size: clamp(30px, 5vw, 56px);
            line-height: 1;
            letter-spacing: -1.6px;
        }

        .intro p {
            color: var(--muted);
            font-size: 16px;
            max-width: 720px;
            line-height: 1.55;
            margin: 0;
        }

        .stats {
            padding: 22px;
            display: grid;
            gap: 14px;
        }

        .stat {
            padding: 16px;
            border-radius: 22px;
            background: rgba(255,255,255,.07);
            border: 1px solid var(--border);
        }

        .stat small {
            color: var(--muted);
            display: block;
            margin-bottom: 8px;
        }

        .stat strong {
            font-size: 28px;
        }

        .setup {
            display: grid;
            grid-template-columns: 1fr 1fr 1fr auto;
            gap: 12px;
            padding: 18px;
            margin-bottom: 20px;
        }

        label {
            display: block;
            font-size: 12px;
            color: var(--muted);
            font-weight: 800;
            text-transform: uppercase;
            letter-spacing: .8px;
            margin-bottom: 8px;
        }

        input, select, button {
            width: 100%;
            border: 0;
            outline: none;
            border-radius: 16px;
            padding: 14px 15px;
            font: inherit;
        }

        input, select {
            background: rgba(255,255,255,.1);
            border: 1px solid var(--border);
            color: var(--text);
        }

        select option {
            background: #111827;
            color: white;
        }

        button {
            cursor: pointer;
            color: #06101d;
            background: linear-gradient(135deg, #38bdf8, #22c55e);
            font-weight: 900;
            box-shadow: 0 16px 34px rgba(34,197,94,.18);
            transition: transform .18s ease, filter .18s ease, opacity .18s ease;
        }

        button:hover {
            transform: translateY(-2px);
            filter: brightness(1.05);
        }

        button:disabled {
            opacity: .45;
            cursor: not-allowed;
            transform: none;
        }

        .exam {
            display: grid;
            grid-template-columns: 320px 1fr;
            gap: 20px;
            align-items: start;
        }

        .sidebar {
            position: sticky;
            top: 16px;
            padding: 18px;
        }

        .progress-wrap {
            height: 12px;
            border-radius: 999px;
            background: rgba(255,255,255,.11);
            overflow: hidden;
            margin: 10px 0 18px;
        }

        .progress {
            height: 100%;
            width: 0%;
            background: linear-gradient(90deg, var(--accent), var(--accent-2));
            border-radius: inherit;
            transition: width .35s ease;
        }

        .mini-grid {
            display: grid;
            grid-template-columns: repeat(5, 1fr);
            gap: 8px;
            margin-top: 14px;
        }

        .mini {
            aspect-ratio: 1;
            border-radius: 12px;
            border: 1px solid var(--border);
            background: rgba(255,255,255,.08);
            color: var(--text);
            display: grid;
            place-items: center;
            font-weight: 900;
            cursor: pointer;
            transition: .2s;
        }

        .mini.active {
            outline: 2px solid var(--accent);
            background: rgba(56,189,248,.17);
        }

        .mini.answered {
            background: rgba(34,197,94,.18);
            border-color: rgba(34,197,94,.45);
        }

        .timer {
            display: flex;
            align-items: center;
            justify-content: space-between;
            padding: 14px;
            border-radius: 18px;
            background: rgba(255,255,255,.08);
            border: 1px solid var(--border);
            margin-top: 14px;
        }

        .timer strong {
            font-size: 24px;
            color: #fef3c7;
        }

        .card {
            padding: 26px;
            min-height: 600px;
        }

        .topline {
            display: flex;
            flex-wrap: wrap;
            gap: 10px;
            align-items: center;
            justify-content: space-between;
            margin-bottom: 18px;
        }

        .chips {
            display: flex;
            gap: 8px;
            flex-wrap: wrap;
        }

        .chip {
            padding: 8px 11px;
            border-radius: 999px;
            background: rgba(255,255,255,.09);
            border: 1px solid var(--border);
            color: #dbeafe;
            font-size: 13px;
            font-weight: 800;
        }

        .question {
            font-size: clamp(22px, 3vw, 34px);
            line-height: 1.18;
            margin: 16px 0 20px;
            letter-spacing: -.4px;
        }

        .options {
            display: grid;
            gap: 12px;
            margin: 22px 0;
        }

        .option {
            display: grid;
            grid-template-columns: 42px 1fr;
            gap: 14px;
            align-items: center;
            padding: 16px;
            border-radius: 22px;
            border: 1px solid var(--border);
            background: rgba(255,255,255,.075);
            cursor: pointer;
            transition: transform .18s ease, background .18s ease, border-color .18s ease;
        }

        .option:hover {
            transform: translateX(4px);
            background: rgba(255,255,255,.12);
        }

        .letter {
            width: 42px;
            height: 42px;
            border-radius: 15px;
            display: grid;
            place-items: center;
            font-weight: 1000;
            background: rgba(255,255,255,.1);
            border: 1px solid var(--border);
        }

        .option.selected {
            border-color: rgba(56,189,248,.9);
            background: rgba(56,189,248,.16);
        }

        .option.correct {
            border-color: rgba(34,197,94,.9);
            background: rgba(34,197,94,.17);
        }

        .option.wrong {
            border-color: rgba(239,68,68,.9);
            background: rgba(239,68,68,.16);
        }

        .feedback {
            display: none;
            padding: 18px;
            border-radius: 22px;
            margin-top: 16px;
            border: 1px solid var(--border);
            background: rgba(255,255,255,.08);
            color: #dbeafe;
            line-height: 1.55;
        }

        .feedback.show {
            display: block;
            animation: pop .25s ease;
        }

        @keyframes pop {
            from { transform: translateY(8px); opacity: 0; }
            to { transform: translateY(0); opacity: 1; }
        }

        .actions {
            display: grid;
            grid-template-columns: 1fr 1fr 1fr;
            gap: 12px;
            margin-top: 18px;
        }

        .secondary {
            background: rgba(255,255,255,.11);
            color: var(--text);
            border: 1px solid var(--border);
            box-shadow: none;
        }

        .danger {
            background: linear-gradient(135deg, #fb7185, #f97316);
            color: #190308;
        }

        .results {
            display: none;
            padding: 26px;
        }

        .results.show {
            display: block;
        }

        .score {
            display: grid;
            grid-template-columns: 220px 1fr;
            gap: 24px;
            align-items: center;
            margin-bottom: 22px;
        }

        .circle {
            width: 220px;
            height: 220px;
            border-radius: 50%;
            display: grid;
            place-items: center;
            background:
                radial-gradient(circle at center, #0f172a 58%, transparent 59%),
                conic-gradient(var(--accent-2) 0deg, var(--accent-2) var(--deg), rgba(255,255,255,.12) var(--deg), rgba(255,255,255,.12) 360deg);
            border: 1px solid var(--border);
        }

        .circle strong {
            font-size: 44px;
        }

        .review {
            display: grid;
            gap: 12px;
            margin-top: 18px;
        }

        .review-item {
            padding: 16px;
            border-radius: 20px;
            background: rgba(255,255,255,.07);
            border: 1px solid var(--border);
        }

        .review-item h3 {
            margin: 0 0 8px;
            font-size: 16px;
        }

        .review-item p {
            color: var(--muted);
            margin: 4px 0;
            line-height: 1.45;
        }

        .hidden {
            display: none !important;
        }

        .floating {
            position: fixed;
            inset: auto 18px 18px auto;
            width: 52px;
            height: 52px;
            border-radius: 18px;
            background: rgba(56,189,248,.18);
            border: 1px solid rgba(56,189,248,.45);
            display: grid;
            place-items: center;
            box-shadow: var(--shadow);
            backdrop-filter: blur(16px);
            animation: float 2.5s ease-in-out infinite;
            z-index: 20;
        }

        @keyframes float {
            0%,100% { transform: translateY(0); }
            50% { transform: translateY(-8px); }
        }

        @media (max-width: 900px) {
            .hero,
            .exam,
            .score {
                grid-template-columns: 1fr;
            }

            .setup {
                grid-template-columns: 1fr;
            }

            .sidebar {
                position: static;
            }

            .actions {
                grid-template-columns: 1fr;
            }

            .circle {
                margin: auto;
            }
        }
    </style>
</head>
<body>
    <div class="floating">💧</div>

    <main class="app">
        <section class="hero">
            <div class="intro glass">
                <span class="badge">⚙️ Examen dinámico de Mecánica de Fluidos</span>
                <h1>Evaluación interactiva con alternativas exigentes</h1>
                <p>
                    Preguntas de análisis conceptual, unidades, presión, masa, peso, compresibilidad,
                    gravedad específica, tensión superficial, flujo incompresible y clasificación de sistemas.
                </p>
            </div>

            <div class="stats glass">
                <div class="stat">
                    <small>Banco de preguntas</small>
                    <strong id="totalBank">30</strong>
                </div>
                <div class="stat">
                    <small>Dificultad</small>
                    <strong>Alta</strong>
                </div>
                <div class="stat">
                    <small>Modo</small>
                    <strong>Dinámico</strong>
                </div>
            </div>
        </section>

        <section class="setup glass" id="setup">
            <div>
                <label>Nombre del estudiante</label>
                <input id="studentName" placeholder="Ingrese nombres y apellidos" />
            </div>

            <div>
                <label>Número de preguntas</label>
                <select id="questionCount">
                    <option value="10">10 preguntas</option>
                    <option value="15" selected>15 preguntas</option>
                    <option value="20">20 preguntas</option>
                    <option value="30">30 preguntas</option>
                </select>
            </div>

            <div>
                <label>Tiempo</label>
                <select id="timeLimit">
                    <option value="10">10 minutos</option>
                    <option value="15" selected>15 minutos</option>
                    <option value="20">20 minutos</option>
                    <option value="30">30 minutos</option>
                </select>
            </div>

            <div style="display:flex;align-items:end;">
                <button onclick="startExam()">Iniciar examen</button>
            </div>
        </section>

        <section class="exam hidden" id="exam">
            <aside class="sidebar glass">
                <label>Progreso</label>
                <div class="progress-wrap">
                    <div class="progress" id="progress"></div>
                </div>

                <div class="stat">
                    <small>Estudiante</small>
                    <strong id="studentLabel" style="font-size:18px;">---</strong>
                </div>

                <div class="timer">
                    <span>⏱ Tiempo</span>
                    <strong id="timer">15:00</strong>
                </div>

                <div class="mini-grid" id="miniGrid"></div>
            </aside>

            <section class="card glass">
                <div class="topline">
                    <div class="chips">
                        <span class="chip" id="qNumber">Pregunta 1</span>
                        <span class="chip" id="qTopic">Tema</span>
                        <span class="chip" id="qLevel">Nivel</span>
                    </div>
                    <span class="chip" id="answeredCount">0 respondidas</span>
                </div>

                <h2 class="question" id="questionText"></h2>

                <div class="options" id="options"></div>

                <div class="feedback" id="feedback"></div>

                <div class="actions">
                    <button class="secondary" onclick="previousQuestion()">Anterior</button>
                    <button onclick="checkQuestion()" id="checkBtn">Verificar</button>
                    <button class="secondary" onclick="nextQuestion()">Siguiente</button>
                </div>

                <div class="actions">
                    <button class="danger" onclick="finishExam()">Finalizar examen</button>
                    <button class="secondary" onclick="toggleReviewMode()">Modo explicación</button>
                    <button class="secondary" onclick="restartExam()">Reiniciar</button>
                </div>
            </section>
        </section>

        <section class="results glass" id="results">
            <div class="score">
                <div class="circle" id="circle" style="--deg:0deg;">
                    <strong id="scorePercent">0%</strong>
                </div>

                <div>
                    <span class="badge">📊 Resultado final</span>
                    <h1 id="finalTitle">Examen finalizado</h1>
                    <p id="finalSummary" style="color:var(--muted);line-height:1.6;"></p>
                    <div class="actions">
                        <button onclick="downloadReport()">Descargar reporte JSON</button>
                        <button class="secondary" onclick="restartExam()">Nuevo intento</button>
                    </div>
                </div>
            </div>

            <div class="review" id="review"></div>
        </section>
    </main>

    <script>
        const QUESTION_BANK = __QUESTIONS__;
        let examQuestions = [];
        let current = 0;
        let answers = {};
        let checked = {};
        let explanations = true;
        let timerInterval = null;
        let remainingSeconds = 0;
        let startTime = null;

        const letters = ["A", "B", "C", "D"];

        document.getElementById("totalBank").textContent = QUESTION_BANK.length;

        function shuffle(array) {
            const copy = [...array];
            for (let i = copy.length - 1; i > 0; i--) {
                const j = Math.floor(Math.random() * (i + 1));
                [copy[i], copy[j]] = [copy[j], copy[i]];
            }
            return copy;
        }

        function prepareQuestions(count) {
            const selected = shuffle(QUESTION_BANK).slice(0, count);
            return selected.map(q => {
                const mapped = q.options.map((text, idx) => ({
                    text,
                    originalIndex: idx,
                    isCorrect: idx === q.answer
                }));

                const mixed = shuffle(mapped);
                const newAnswer = mixed.findIndex(opt => opt.isCorrect);

                return {
                    ...q,
                    options: mixed.map(opt => opt.text),
                    answer: newAnswer
                };
            });
        }

        function startExam() {
            const name = document.getElementById("studentName").value.trim();
            const count = parseInt(document.getElementById("questionCount").value);
            const minutes = parseInt(document.getElementById("timeLimit").value);

            if (!name) {
                alert("Ingrese el nombre del estudiante.");
                return;
            }

            examQuestions = prepareQuestions(count);
            current = 0;
            answers = {};
            checked = {};
            explanations = true;
            startTime = new Date().toISOString();
            remainingSeconds = minutes * 60;

            document.getElementById("studentLabel").textContent = name;
            document.getElementById("setup").classList.add("hidden");
            document.getElementById("results").classList.remove("show");
            document.getElementById("exam").classList.remove("hidden");

            buildMiniGrid();
            renderQuestion();
            startTimer();
        }

        function startTimer() {
            clearInterval(timerInterval);
            updateTimer();

            timerInterval = setInterval(() => {
                remainingSeconds--;
                updateTimer();

                if (remainingSeconds <= 0) {
                    clearInterval(timerInterval);
                    finishExam();
                }
            }, 1000);
        }

        function updateTimer() {
            const min = Math.floor(Math.max(0, remainingSeconds) / 60);
            const sec = Math.max(0, remainingSeconds) % 60;
            document.getElementById("timer").textContent =
                String(min).padStart(2, "0") + ":" + String(sec).padStart(2, "0");
        }

        function buildMiniGrid() {
            const grid = document.getElementById("miniGrid");
            grid.innerHTML = "";

            examQuestions.forEach((_, idx) => {
                const div = document.createElement("div");
                div.className = "mini";
                div.textContent = idx + 1;
                div.onclick = () => {
                    current = idx;
                    renderQuestion();
                };
                grid.appendChild(div);
            });
        }

        function renderQuestion() {
            const q = examQuestions[current];

            document.getElementById("qNumber").textContent = `Pregunta ${current + 1} de ${examQuestions.length}`;
            document.getElementById("qTopic").textContent = q.topic;
            document.getElementById("qLevel").textContent = q.level;
            document.getElementById("questionText").textContent = q.question;

            const options = document.getElementById("options");
            options.innerHTML = "";

            q.options.forEach((option, idx) => {
                const div = document.createElement("div");
                div.className = "option";

                if (answers[q.id] === idx) div.classList.add("selected");

                if (checked[q.id]) {
                    if (idx === q.answer) div.classList.add("correct");
                    if (answers[q.id] === idx && idx !== q.answer) div.classList.add("wrong");
                }

                div.onclick = () => selectOption(idx);

                div.innerHTML = `
                    <div class="letter">${letters[idx]}</div>
                    <div>${option}</div>
                `;

                options.appendChild(div);
            });

            const feedback = document.getElementById("feedback");
            if (checked[q.id] && explanations) {
                const ok = answers[q.id] === q.answer;
                feedback.classList.add("show");
                feedback.innerHTML = `
                    <strong>${ok ? "✅ Correcto" : "❌ Incorrecto"}</strong><br>
                    <span>${q.explanation}</span>
                `;
            } else {
                feedback.classList.remove("show");
                feedback.innerHTML = "";
            }

            updateProgress();
            updateMiniGrid();
        }

        function selectOption(idx) {
            const q = examQuestions[current];
            if (checked[q.id]) return;
            answers[q.id] = idx;
            renderQuestion();
        }

        function checkQuestion() {
            const q = examQuestions[current];

            if (answers[q.id] === undefined) {
                alert("Seleccione una alternativa antes de verificar.");
                return;
            }

            checked[q.id] = true;
            renderQuestion();
        }

        function previousQuestion() {
            if (current > 0) {
                current--;
                renderQuestion();
            }
        }

        function nextQuestion() {
            if (current < examQuestions.length - 1) {
                current++;
                renderQuestion();
            }
        }

        function toggleReviewMode() {
            explanations = !explanations;
            renderQuestion();
        }

        function updateProgress() {
            const answered = Object.keys(answers).length;
            const percent = (answered / examQuestions.length) * 100;
            document.getElementById("progress").style.width = percent + "%";
            document.getElementById("answeredCount").textContent = `${answered} respondidas`;
        }

        function updateMiniGrid() {
            const minis = document.querySelectorAll(".mini");

            minis.forEach((mini, idx) => {
                mini.classList.toggle("active", idx === current);
                mini.classList.toggle("answered", answers[examQuestions[idx].id] !== undefined);
            });
        }

        function calculateScore() {
            let correct = 0;

            examQuestions.forEach(q => {
                if (answers[q.id] === q.answer) correct++;
            });

            const total = examQuestions.length;
            const percent = Math.round((correct / total) * 100);
            const vigesimal = ((correct / total) * 20).toFixed(2);

            return { correct, total, percent, vigesimal };
        }

        function finishExam() {
            clearInterval(timerInterval);

            const result = calculateScore();
            const name = document.getElementById("studentName").value.trim();
            const endTime = new Date().toISOString();

            document.getElementById("exam").classList.add("hidden");
            document.getElementById("results").classList.add("show");

            document.getElementById("scorePercent").textContent = `${result.percent}%`;
            document.getElementById("circle").style.setProperty("--deg", `${result.percent * 3.6}deg`);

            let status = "Necesita reforzamiento.";
            if (result.percent >= 85) status = "Excelente dominio conceptual y operativo.";
            else if (result.percent >= 70) status = "Buen desempeño, con algunos puntos por afinar.";
            else if (result.percent >= 55) status = "Desempeño regular; requiere revisar fundamentos.";

            document.getElementById("finalTitle").textContent = `${name}, nota: ${result.vigesimal}/20`;
            document.getElementById("finalSummary").innerHTML = `
                Correctas: <strong>${result.correct}</strong> de <strong>${result.total}</strong>.
                Porcentaje: <strong>${result.percent}%</strong>. ${status}<br>
                Inicio: ${formatDate(startTime)} | Fin: ${formatDate(endTime)}
            `;

            buildReview();
        }

        function buildReview() {
            const review = document.getElementById("review");
            review.innerHTML = "";

            examQuestions.forEach((q, idx) => {
                const selected = answers[q.id];
                const ok = selected === q.answer;

                const item = document.createElement("div");
                item.className = "review-item";

                item.innerHTML = `
                    <h3>${ok ? "✅" : "❌"} ${idx + 1}. ${q.question}</h3>
                    <p><strong>Tu respuesta:</strong> ${selected === undefined ? "Sin responder" : letters[selected] + ". " + q.options[selected]}</p>
                    <p><strong>Respuesta correcta:</strong> ${letters[q.answer]}. ${q.options[q.answer]}</p>
                    <p><strong>Fundamento:</strong> ${q.explanation}</p>
                `;

                review.appendChild(item);
            });
        }

        function downloadReport() {
            const result = calculateScore();
            const name = document.getElementById("studentName").value.trim();

            const report = {
                estudiante: name,
                fecha: new Date().toISOString(),
                puntaje: {
                    correctas: result.correct,
                    total: result.total,
                    porcentaje: result.percent,
                    nota_vigesimal: result.vigesimal
                },
                respuestas: examQuestions.map((q, idx) => ({
                    numero: idx + 1,
                    tema: q.topic,
                    nivel: q.level,
                    pregunta: q.question,
                    respuesta_estudiante: answers[q.id] === undefined ? null : q.options[answers[q.id]],
                    respuesta_correcta: q.options[q.answer],
                    correcto: answers[q.id] === q.answer,
                    explicacion: q.explanation
                }))
            };

            const blob = new Blob([JSON.stringify(report, null, 2)], { type: "application/json" });
            const url = URL.createObjectURL(blob);
            const a = document.createElement("a");
            a.href = url;
            a.download = `reporte_examen_${name.replaceAll(" ", "_").toLowerCase()}.json`;
            a.click();
            URL.revokeObjectURL(url);
        }

        function restartExam() {
            clearInterval(timerInterval);
            document.getElementById("setup").classList.remove("hidden");
            document.getElementById("exam").classList.add("hidden");
            document.getElementById("results").classList.remove("show");
        }

        function formatDate(iso) {
            if (!iso) return "---";
            const d = new Date(iso);
            return d.toLocaleString("es-PE", {
                year: "numeric",
                month: "2-digit",
                day: "2-digit",
                hour: "2-digit",
                minute: "2-digit"
            });
        }
    </script>
</body>
</html>
"""

@app.route("/")
def index():
    html = HTML.replace("__QUESTIONS__", str(QUESTIONS).replace("True", "true").replace("False", "false").replace("None", "null"))
    return render_template_string(html)

@app.route("/api/questions")
def api_questions():
    return jsonify(QUESTIONS)

@app.route("/api/grade", methods=["POST"])
def api_grade():
    data = request.get_json(force=True)
    submitted = data.get("answers", {})
    correct = 0
    total = len(QUESTIONS)

    answer_key = {str(q["id"]): q["answer"] for q in QUESTIONS}

    for qid, answer in submitted.items():
        if str(qid) in answer_key and int(answer) == answer_key[str(qid)]:
            correct += 1

    return jsonify({
        "correct": correct,
        "total": total,
        "percent": round((correct / total) * 100, 2),
        "score_20": round((correct / total) * 20, 2),
        "graded_at": datetime.now().isoformat()
    })

if __name__ == "__main__":
    app.run(debug=True)
