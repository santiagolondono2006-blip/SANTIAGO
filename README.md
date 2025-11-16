<!DOCTYPE html>
<html lang="es" class="scroll-smooth">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Bitácora de Proyecto - Easy Global</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;700&display=swap" rel="stylesheet">
    
    <!-- Chosen Palette: Warm Neutral & Sky Blue -->
    <!-- Application Structure Plan: He diseñado una SPA temática y orientada al proceso. La navegación principal no es una lista de fechas, sino un "Stepper" interactivo de 8 pasos que representa la "Ruta de la Solución Tecnológica". Al hacer clic en una fase (ej. "Lluvia de Ideas"), JavaScript carga dinámicamente el contenido de esa entrada del diario en un área de contenido central. Esta arquitectura es superior a un simple scroll, ya que refleja la metodología estructurada del informe, permitiendo al usuario explorar cada fase del proyecto (el problema, el modelo de negocio, la tecnología) de forma enfocada e interactiva. -->
    <!-- Visualization & Content Choices:
        - Report Info: 8 Fases de la "Ruta" -> Goal: Navegación -> Viz: Stepper Interactivo (HTML/Tailwind/JS) -> Interaction: Clic carga contenido -> Justification: Refleja la metodología central del informe.
        - Report Info: 4 Pasos del Flujo de Usuario (Fase 3) -> Goal: Organizar -> Viz: Diagrama de Flujo (HTML/Tailwind Flexbox) -> Interaction: Estático -> Justification: Claro, visual y sin SVG.
        - Report Info: Comisiones % (Fase 4) -> Goal: Informar -> Viz: Gráfico Doughnut (Chart.js) -> Interaction: Hover -> Justification: Visualiza los únicos datos cuantitativos clave (comisiones).
        - Report Info: 4 Pilares de Verificación (Fase 5) -> Goal: Organizar -> Viz: Tarjetas en Grid (HTML/Tailwind) -> Interaction: Estático -> Justification: Agrupación clara de conceptos.
        - Report Info: Tech Stack (Fase 8) -> Goal: Informar -> Viz: Tarjetas con Iconos Unicode (HTML/Tailwind) -> Interaction: Estático -> Justification: Resumen visual de la tecnología sin SVG.
    -->
    <!-- CONFIRMATION: NO SVG graphics used. NO Mermaid JS used. -->

    <style>
        body {
            font-family: 'Inter', sans-serif;
            background-color: #f3f4f6; /* bg-gray-100 */
        }
        
        .stepper-item {
            transition: all 0.3s ease;
            cursor: pointer;
        }
        
        .stepper-item:hover .stepper-circle {
            background-color: #0284c7; /* sky-600 */
            color: white;
        }

        .stepper-item.active .stepper-circle {
            background-color: #0369a1; /* sky-700 */
            color: white;
            font-weight: bold;
        }
        
        .stepper-item.active .stepper-label {
            color: #0369a1; /* sky-700 */
            font-weight: 700;
        }
        
        .stepper-line {
            height: 2px;
            background-color: #d1d5db; /* gray-300 */
            flex-grow: 1;
            margin-top: 1.25rem; /* mt-5 */
        }
        
        .chart-container {
            position: relative;
            width: 100%;
            max-width: 400px;
            height: 400px;
            max-height: 400px;
            margin-left: auto;
            margin-right: auto;
        }
        
        @media (max-width: 640px) {
            .stepper-label {
                font-size: 0.75rem; /* text-xs */
                text-align: center;
                max-width: 60px;
            }
            .stepper-circle {
                width: 2rem; /* w-8 */
                height: 2rem; /* h-8 */
                font-size: 0.875rem; /* text-sm */
            }
            .stepper-line {
                 margin-top: 0.75rem; /* mt-3 */
            }
        }

    </style>
</head>
<body class="text-gray-800">

    <!-- Header -->
    <header class="bg-white shadow-md sticky top-0 z-50">
        <nav class="container mx-auto px-4 sm:px-6 lg:px-8 py-4">
            <h1 class="text-2xl md:text-3xl font-bold text-sky-700">Bitácora Interactiva: Proyecto Easy Global</h1>
            <p class="text-sm md:text-base text-gray-600">De DAWN ENERGY a un Marketplace de Servicios</p>
        </nav>
    </header>

    <!-- Main Content -->
    <main class="container mx-auto p-4 sm:p-6 lg:p-8">

        <!-- Sección de Información -->
        <section class="bg-white p-6 rounded-xl shadow-lg mb-8">
            <div class="flex flex-col md:flex-row md:items-center md:justify-between">
                <div>
                    <h2 class="text-xl font-bold text-gray-900">Santiago Londoño Orjuela</h2>
                    <p class="text-gray-600">Estudiante, Trend+Tech</p>
                </div>
                <div class="mt-4 md:mt-0 md:text-right">
                    <p class="text-gray-600"><span class="font-semibold">Caso de Estudio:</span> DAWN ENERGY</p>
                    <p class="text-gray-600"><span class="font-semibold">Periodo:</span> 22 Oct - 14 Nov 2025</p>
                </div>
            </div>
        </section>

        <!-- Navegador Interactivo de Fases -->
        <section class="bg-white p-6 rounded-xl shadow-lg mb-8">
            <h3 class="text-xl font-bold text-center mb-6 text-sky-700">Ruta de la Solución Tecnológica</h3>
            <div id="stepper-container" class="flex justify-between items-start">
                <!-- Los 8 pasos se generarán con JS -->
            </div>
        </section>

        <!-- Área de Contenido Dinámico -->
        <section id="dynamic-content-container" class="bg-white p-6 sm:p-8 rounded-xl shadow-lg min-h-[500px]">
            <!-- El contenido de la fase se cargará aquí -->
            <div id="dynamic-content" class="opacity-0 transition-opacity duration-500">
                <!-- Placeholder mientras carga -->
                <div class="flex justify-center items-center h-96">
                    <p class="text-gray-500 text-lg">Selecciona una fase para ver los detalles.</p>
                </div>
            </div>
        </section>

    </main>

    <!-- Footer -->
    <footer class="text-center py-6 mt-8">
        <p class="text-gray-500 text-sm">&copy; 2025 Santiago Londoño Orjuela - Proyecto Trend+Tech</p>
    </footer>

    <script>
        document.addEventListener('DOMContentLoaded', () => {
            
            const diaryData = [
                {
                    fecha: "22 de octubre de 2025",
                    fase: "Fase 1",
                    titulo: "Búsqueda de los Problemas",
                    actividad: "El \"Muro de la Instalación\": Descubrimiento del límite de escalabilidad en el modelo online.",
                    bitacora: "Hoy se sintió como chocar contra un muro. Revisando las métricas, confirmamos que, aunque DAWN ENERGY vende exitosamente kits solares básicos online, fracasa en la instalación de proyectos grandes. El problema no es la demanda, ¡es la mano de obra física! Me di cuenta de que mi rol como empresario debe cambiar: no puedo seguir atado a un modelo de negocio que ignora la geografía. La frustración se convirtió en la pregunta: ¿Cómo se resuelve el caos de la mano de obra presencial? Si no puedo internalizarla, debo crear un modelo de negocio nuevo que la ordene.",
                    objetivo: "Aislar la problemática central de DAWN ENERGY para transformarla en una oportunidad de negocio paralela.",
                    logro: "Se identificó que la verdadera oportunidad era la intermediación de servicios de confianza que el mercado actual no ofrece, validando la necesidad de un nuevo proyecto.",
                    desafio: "Superar la inercia del modelo de negocio actual y asumir el riesgo de pivotar la energía mental a una solución completamente ajena.",
                    solucion: "Definición del Proyecto Easy Global como una entidad independiente y enfocada en la tercerización y verificación de servicios."
                },
                {
                    fecha: "23 de octubre de 2025",
                    fase: "Fase 2",
                    titulo: "Contexto del Problema",
                    actividad: "Conceptualización del Marketplace de Confianza y validación de la GIG Economy.",
                    bitacora: "La idea tomó forma rápidamente: un marketplace. La conversación con colegas fue el catalizador. Me di cuenta de que el problema no es la escasez de técnicos (hay electricistas SENA, plomeros, etc.), sino la escasez de confianza en ellos. Easy Global debe ser el escudo que protege tanto al cliente como al profesional. Hoy definimos el rol clave del proyecto: ser el generador de confianza a través de la verificación rigurosa, lo que nos permite entrar en la lucrativa economía GIG de servicios presenciales.",
                    objetivo: "Confirmar la existencia de una masa crítica de técnicos cualificados y que Easy Global sea percibido como un valor, no solo un costo.",
                    logro: "La validación inicial confirmó la alta disposición de los profesionales a pagar una comisión a cambio de trabajo seguro y verificado. El concepto del servicio premium de verificación se convirtió en el diferenciador.",
                    desafio: "Diseñar un sistema de verificación que sea infalible contra el fraude pero ágil para no frustrar a los técnicos.",
                    solucion: "Definir el servicio premium de verificación (doble filtro: documental y antecedentes) como el pilar de la plataforma."
                },
                {
                    fecha: "24 de octubre de 2025",
                    fase: "Fase 3",
                    titulo: "Pregunta Problema",
                    actividad: "El Flujo del Dinero y la Seguridad: Diseño del Customer Journey (MVP).",
                    bitacora: "Hoy definimos el cómo. La gran pregunta era: ¿Cómo garantizamos el pago al técnico y el cumplimiento del servicio al cliente? La respuesta fue el sistema Escrow. Diseñamos un flujo que prioriza la seguridad y la experiencia de usuario (UX). Un cliente no puede buscar eternamente; debe ser rápido y local. Se establecieron los 4 pasos clave: Publicar, Seleccionar, Ejecutar, Pagar/Calificar. Este flujo simple es la base de nuestro Producto Mínimo Viable (MVP). La búsqueda de una API de geolocalización eficiente se convirtió en nuestro primer reto técnico real.",
                    objetivo: "Estructurar los pasos mínimos viables (MVP) para conectar al cliente con el técnico de forma segura.",
                    logro: "Definición del flujo de 4 pasos para la transacción y la implementación del pago seguro en depósito (Escrow).",
                    desafio: "Identificar la API de geolocalización adecuada para el matchmaking eficiente y la implementación del sistema Escrow en el MVP.",
                    solucion: "Se exploran APIs de terceros (como Google Maps) para la funcionalidad de cercanía."
                },
                {
                    fecha: "27 de octubre de 2025",
                    fase: "Fase 4",
                    titulo: "Lluvia de Ideas",
                    actividad: "La Misión y la Comisión: Diseño del Motor de Crecimiento del Proyecto.",
                    bitacora: "El éxito de Easy Global dependía de que los técnicos vieran en la plataforma un aliado, no un jefe. Después de una intensa \"lluvia de ideas\" de modelos de monetización, desechamos las tarifas fijas. La comisión tenía que ser justa. Diseñamos un modelo híbrido: una comisión base mínima (5-7%) para la intermediación segura, y el gran diferenciador: el Premium Listing (10%). Convencer a un técnico de pagar el 10% por visibilidad es el reto, pero lo replanteamos: no es un costo, es una inversión garantizada en *leads*. Hoy solidificamos el motor financiero.",
                    objetivo: "Definir una estructura de comisiones que se sienta justa y de valor agregado para el profesional.",
                    logro: "Se estableció el modelo híbrido: Comisión Estándar (5-7%) por la intermediación y seguridad, y Premium Listing (10% de comisión) por visibilidad prioritaria y *leads* calificados.",
                    desafio: "Vender el concepto de Premium Listing a profesionales acostumbrados a trabajar de boca en boca.",
                    solucion: "Desarrollar una narrativa de valor clara: el 10% es una inversión en marketing digital que asegura un mejor retorno."
                },
                {
                    fecha: "28 de octubre de 2025",
                    fase: "Fase 5",
                    titulo: "Encuesta para la Selección de Ideas",
                    actividad: "El Núcleo del Negocio (3 Días de Trabajo Intenso): Ingeniería de la Confianza, Legalidad y Estrategia Anti-Evasión.",
                    bitacora: "Esta fue una semana de inmersión total. El 28, 29 y 30 fueron un torbellino de decisiones estratégicas. El trabajo se centró en la Selección Final de Ideas y el blindaje del modelo. Sabía que la idea sería un fracaso si no podíamos resolver tres problemas existenciales: 1. Confianza Total (El Producto): Definir el onboarding centrado en los 4 Pilares de la Verificación para que nuestra Ficha de Perfil fuera el currículum más confiable del mercado. 2. Blindaje Legal (La Defensa): Explorar la figura legal de intermediario para no heredar responsabilidades laborales de los técnicos, justificando nuestra comisión. 3. Anti-Evasión (La Supervivencia): Diseñar la estrategia para evitar la fuga de transacciones. La solución fue la Garantía Post-Servicio (solo dentro de Easy Global) y el control de la información de contacto. El riesgo era real, pero encontramos una forma de construir un ecosistema donde valga la pena quedarse.",
                    objetivo: "Poner a prueba la viabilidad estratégica del proyecto resolviendo sus tres mayores riesgos: Confianza, Ley y Fuga de Transacciones.",
                    logro: "Definición de los 4 Pilares de la Verificación y la estrategia de Fidelización por Garantía. El modelo legal de \"intermediario\" fue validado como la ruta a seguir.",
                    desafio: "El gran riesgo de un marketplace presencial: la fuga de transacciones (secuestro de clientes) y la complejidad de la legislación laboral para contratistas independientes.",
                    solucion: "Diseño de la UX para asegurar que la información de contacto inicial del técnico solo se libere una vez la transacción esté asegurada."
                },
                {
                    fecha: "31 de octubre de 2025",
                    fase: "Fase 6",
                    titulo: "Definición del Alcance Masivo",
                    actividad: "El Gran Salto Conceptual: De Nicho de Energía a \"Fiverr Presencial\".",
                    bitacora: "Hoy fue el día de la visión ilimitada. Revisando la taxonomía del servicio, caí en la cuenta de que la Confianza y el Escrow son escalables a *cualquier* oficio presencial. El problema logístico de DAWN ENERGY era solo el punto de partida. Easy Global no era solo para paneles solares; podía ser el \"Fiverr Presencial\" para plomería, electricidad, construcción, etc. Este cambio elevó el proyecto a un mercado gigantesco. Para soportar esta escalabilidad, reestructuré la aplicación: en lugar de buscar por \"oficio\", el cliente buscará por el \"Problema a Resolver\". Esta reestructuración es un prototipo conceptual en papel.",
                    objetivo: "Reestructurar la taxonomía de la aplicación para soportar una escalabilidad ilimitada, consolidando la visión del proyecto.",
                    logro: "Identificación de la escalabilidad ilimitada del modelo. El proyecto Easy Global se consolida con esta visión masiva, abandonando la limitación inicial de DAWN ENERGY.",
                    desafio: "Simplificar la interfaz de búsqueda para que el cliente no se sienta abrumado por la cantidad de categorías disponibles.",
                    solucion: "Propuesta de un sistema de clasificación jerárquica por \"Problema a Resolver\"."
                },
                {
                    fecha: "10 de noviembre de 2025",
                    fase: "Fase 7",
                    titulo: "Diseño Conceptual y Búsqueda de Recursos",
                    actividad: "Formalización del MVP Conceptual y Búsqueda de Talentos Técnicos.",
                    bitacora: "Cerramos la fase estratégica. El modelo está claro y la visión ambiciosa. Hoy me dediqué a formalizar los pasos a seguir. ¿Cuál es el próximo desafío? La ejecución. Necesitamos un diseño conceptual detallado antes de empezar a escribir código. Mi objetivo inmediato es conseguir el *talento full-stack* para construir el MVP. El mejor recurso está cerca: mi hermano, que se unirá para la arquitectura técnica. Es el fin de la estrategia pura y el comienzo del *hacer*, lo que nos llevará a la fase de investigación de herramientas.",
                    objetivo: "Formalizar el roadmap de implementación (MVP) e iniciar la búsqueda de recursos técnicos.",
                    logro: "Definición de los requerimientos para el Diseño Conceptual (Wireframes) y el alcance del MVP. La arquitectura de datos clave fue identificada.",
                    desafio: "La necesidad de integrar habilidades de desarrollo full-stack para dar vida al proyecto de manera ágil.",
                    solucion: "Atracción de talento técnico: Se inicia la colaboración con mi hermano programador para avanzar a la fase técnica."
                },
                {
                    fecha: "14 de noviembre de 2025",
                    fase: "Fase 8",
                    titulo: "Investigación de las herramientas para crear la solución",
                    actividad: "Investigación de las Herramientas: Definiendo la Pila Tecnológica (Tech Stack) para Easy Global.",
                    bitacora: "Con el plan de negocios validado, el enfoque pasó a la tecnología. Hoy investigamos la *pila tecnológica* ideal para un marketplace geolocalizado con pagos seguros. La decisión crítica es la escalabilidad futura y la necesidad de una aplicación móvil. Evaluamos frameworks *full-stack* como React Native (por la necesidad de app móvil y web desde el mismo código), Node.js/Express para el backend (por su velocidad y ecosistema), y Firebase/Firestore para la base de datos (por su escalabilidad en tiempo real para las búsquedas geolocalizadas). También investigamos expertos en seguridad de pagos (Escrow) y APIs de geolocalización robustas. La conclusión es clara: la solución requiere este stack moderno y ágil que ahora debemos dominar o consultar.",
                    objetivo: "Seleccionar la pila tecnológica más eficiente, escalable y robusta para la construcción del MVP de Easy Global.",
                    logro: "Propuesta inicial de Tech Stack (React Native, Node.js/Express, Firebase/Firestore). Identificación de la necesidad de consultar a expertos en seguridad y legislación financiera para el módulo de Escrow.",
                    desafio: "Conciliar la necesidad de velocidad de desarrollo con la complejidad de implementar sistemas de pago y geolocalización en tiempo real.",
                    solucion: "Priorización de la adquisición de habilidades en la Tech Stack seleccionada y programación de consultas con asesores legales/financieros para el manejo del dinero en depósito."
                }
            ];

            const stepperContainer = document.getElementById('stepper-container');
            const contentEl = document.getElementById('dynamic-content');
            const contentContainer = document.getElementById('dynamic-content-container');
            
            let currentChart = null;

            function buildStepper() {
                diaryData.forEach((data, index) => {
                    const stepEl = document.createElement('div');
                    stepEl.classList.add('stepper-item', 'flex', 'flex-col', 'items-center', 'relative');
                    stepEl.innerHTML = `
                        <div class="stepper-circle w-10 h-10 sm:w-12 sm:h-12 flex items-center justify-center rounded-full bg-gray-200 text-gray-600 z-10 text-lg sm:text-xl">
                            ${index + 1}
                        </div>
                        <span class="stepper-label text-gray-500 mt-2 text-xs sm:text-sm text-center">${data.titulo}</span>
                    `;
                    stepEl.addEventListener('click', () => renderPhase(index));
                    stepperContainer.appendChild(stepEl);

                    if (index < diaryData.length - 1) {
                        const lineEl = document.createElement('div');
                        lineEl.classList.add('stepper-line');
                        stepperContainer.appendChild(lineEl);
                    }
                });
            }

            function renderPhase(phaseIndex) {
                if (currentChart) {
                    currentChart.destroy();
                    currentChart = null;
                }
                
                contentEl.classList.add('opacity-0');

                setTimeout(() => {
                    const data = diaryData[phaseIndex];
                    let contentHTML = `
                        <div class="mb-4">
                            <span class="inline-block bg-sky-100 text-sky-700 px-3 py-1 rounded-full text-sm font-semibold">${data.fase}</span>
                            <span class="ml-2 text-gray-500 text-sm">${data.fecha}</span>
                        </div>
                        <h2 class="text-2xl md:text-3xl font-bold mb-4">${data.actividad}</h2>
                        
                        <div class="border-l-4 border-sky-500 pl-4 mb-6">
                            <h4 class="text-lg font-semibold text-gray-700">Bitácora del Empresario:</h4>
                            <p class="text-gray-600 italic">"${data.bitacora}"</p>
                        </div>
                        
                        <div class="grid grid-cols-1 md:grid-cols-2 gap-6 mb-6">
                            <div class="bg-gray-50 p-4 rounded-lg border border-gray-200">
                                <h5 class="font-bold text-gray-800 mb-2">🎯 Objetivo del Día</h5>
                                <p class="text-gray-600">${data.objetivo}</p>
                            </div>
                            <div class="bg-gray-50 p-4 rounded-lg border border-gray-200">
                                <h5 class="font-bold text-gray-800 mb-2">🏆 Logro Alcanzado</h5>
                                <p class="text-gray-600">${data.logro}</p>
                            </div>
                            <div class="bg-gray-50 p-4 rounded-lg border border-gray-200">
                                <h5 class="font-bold text-gray-800 mb-2">🧗 Desafío Encontrado</h5>
                                <p class="text-gray-600">${data.desafio}</p>
                            </div>
                            <div class="bg-gray-50 p-4 rounded-lg border border-gray-200">
                                <h5 class="font-bold text-gray-800 mb-2">💡 Solución Implementada</h5>
                                <p class="text-gray-600">${data.solucion}</p>
                            </div>
                        </div>
                        
                        <div id="visualization-area" class="mt-8"></div>
                    `;
                    
                    contentEl.innerHTML = contentHTML;
                    
                    const vizArea = document.getElementById('visualization-area');
                    
                    if (phaseIndex === 2) { // Fase 3: Pregunta Problema
                        vizArea.innerHTML = `
                            <h4 class="text-xl font-bold text-center mb-4 text-gray-800">Flujo de Usuario (MVP)</h4>
                            <div class="flex flex-col sm:flex-row justify-around items-center space-y-4 sm:space-y-0 sm:space-x-4">
                                ${createFlowStep('1. Publicar', 'Publicar necesidad geolocalizada')}
                                <span class="text-sky-600 font-bold text-2xl hidden sm:block">→</span>
                                ${createFlowStep('2. Seleccionar', 'Seleccionar técnico verificado')}
                                <span class="text-sky-600 font-bold text-2xl hidden sm:block">→</span>
                                ${createFlowStep('3. Ejecutar', 'Ejecutar servicio y validar')}
                                <span class="text-sky-600 font-bold text-2xl hidden sm:block">→</span>
                                ${createFlowStep('4. Pagar', 'Liberar pago (Escrow) y calificar')}
                            </div>
                        `;
                    }
                    
                    if (phaseIndex === 3) { // Fase 4: Lluvia de Ideas
                        vizArea.innerHTML = `
                            <h4 class="text-xl font-bold text-center mb-4 text-gray-800">Modelo de Monetización</h4>
                            <div class="flex flex-col lg:flex-row items-center gap-6">
                                <div class="flex-1 w-full">
                                    <div class="chart-container">
                                        <canvas id="monetizationChart"></canvas>
                                    </div>
                                    <p class="text-center text-sm text-gray-500 mt-2">Distribución de Ingreso (Modelo Estándar 7%)</p>
                                </div>
                                <div class="flex-1 w-full space-y-4">
                                    <div class="bg-sky-50 p-4 rounded-lg border border-sky-200 text-center">
                                        <h5 class="font-bold text-sky-700 text-lg">Comisión Estándar</h5>
                                        <p class="text-3xl font-bold text-sky-600">5% - 7%</p>
                                        <p class="text-gray-600">Por intermediación, seguridad y garantía.</p>
                                    </div>
                                    <div class="bg-emerald-50 p-4 rounded-lg border border-emerald-200 text-center">
                                        <h5 class="font-bold text-emerald-700 text-lg">Premium Listing</h5>
                                        <p class="text-3xl font-bold text-emerald-600">10%</p>
                                        <p class="text-gray-600">Por visibilidad prioritaria y *leads* calificados.</p>
                                    </div>
                                </div>
                            </div>
                        `;
                        createMonetizationChart();
                    }
                    
                    if (phaseIndex === 4) { // Fase 5: Encuesta
                        vizArea.innerHTML = `
                            <h4 class="text-xl font-bold text-center mb-4 text-gray-800">Ingeniería de la Confianza y Riesgos</h4>
                            <div class="grid grid-cols-1 md:grid-cols-3 gap-4">
                                <div class="bg-gray-50 p-4 rounded-lg border border-gray-200">
                                    <h5 class="font-bold text-gray-800 mb-2">1. Confianza Total</h5>
                                    <ul class="list-disc list-inside text-gray-600">
                                        <li>Pilar 1: Identidad</li>
                                        <li>Pilar 2: Experiencia</li>
                                        <li>Pilar 3: Certificación</li>
                                        <li>Pilar 4: Calidad (Calificaciones)</li>
                                    </ul>
                                </div>
                                <div class="bg-gray-50 p-4 rounded-lg border border-gray-200">
                                    <h5 class="font-bold text-gray-800 mb-2">2. Blindaje Legal</h5>
                                    <p class="text-gray-600">Definir el proyecto como "intermediario" bajo el modelo GIG Economy para evitar responsabilidad laboral.</p>
                                </div>
                                <div class="bg-gray-50 p-4 rounded-lg border border-gray-200">
                                    <h5 class="font-bold text-gray-800 mb-2">3. Estrategia Anti-Evasión</h5>
                                    <p class="text-gray-600">Ofrecer "Garantía Post-Servicio" y controlar la información de contacto para fidelizar y proteger la transacción.</p>
                                </div>
                            </div>
                        `;
                    }
                    
                    if (phaseIndex === 5) { // Fase 6: Alcance
                         vizArea.innerHTML = `
                            <h4 class="text-xl font-bold text-center mb-4 text-gray-800">El Gran Salto Conceptual</h4>
                            <div class="flex flex-col sm:flex-row items-center justify-center space-x-0 sm:space-x-6 space-y-4 sm:space-y-0">
                                <div class="border-2 border-red-300 bg-red-50 p-6 rounded-xl text-center">
                                    <h5 class="font-bold text-red-700">Visión Anterior</h5>
                                    <p class="text-red-600">Nicho de Energía Solar</p>
                                </div>
                                <span class="text-sky-600 font-bold text-4xl transform rotate-90 sm:rotate-0">→</span>
                                <div class="border-2 border-green-300 bg-green-50 p-6 rounded-xl text-center shadow-xl">
                                    <h5 class="font-bold text-green-700">Visión Masiva</h5>
                                    <p class="text-2xl font-bold text-green-600">"Fiverr Presencial"</p>
                                    <p class="text-gray-600">(Plomería, Electricidad, Construcción, etc.)</p>
                                </div>
                            </div>
                        `;
                    }
                    
                    if (phaseIndex === 7) { // Fase 8: Herramientas
                        vizArea.innerHTML = `
                            <h4 class="text-xl font-bold text-center mb-4 text-gray-800">Pila Tecnológica (Tech Stack) Propuesta</h4>
                            <div class="grid grid-cols-1 sm:grid-cols-3 gap-6">
                                ${createTechCard('📱', 'React Native', 'Para app móvil y web desde un solo código.')}
                                ${createTechCard('⚙️', 'Node.js / Express', 'Para un backend rápido, escalable y en tiempo real.')}
                                ${createTechCard('🗃️', 'Firebase / Firestore', 'Para base de datos en tiempo real y búsquedas geolocalizadas.')}
                            </div>
                        `;
                    }

                    contentEl.classList.remove('opacity-0');
                    
                    stepperContainer.querySelectorAll('.stepper-item').forEach((item, index) => {
                        if (index === phaseIndex) {
                            item.classList.add('active');
                        } else {
                            item.classList.remove('active');
                        }
                    });
                }, 150);
            }
            
            function createFlowStep(title, text) {
                return `
                <div class="bg-white border-2 border-gray-200 p-4 rounded-lg text-center w-48 h-36 flex flex-col justify-center">
                    <h6 class="font-bold text-sky-700">${title}</h6>
                    <p class="text-sm text-gray-600">${text}</p>
                </div>
                `;
            }
            
            function createTechCard(icon, title, text) {
                return `
                <div class="bg-gray-50 p-6 rounded-lg border border-gray-200 text-center shadow-lg">
                    <div class="text-6xl mb-4">${icon}</div>
                    <h5 class="font-bold text-gray-800 text-xl mb-2">${title}</h5>
                    <p class="text-gray-600">${text}</p>
                </div>
                `;
            }

            function createMonetizationChart() {
                const ctx = document.getElementById('monetizationChart').getContext('2d');
                currentChart = new Chart(ctx, {
                    type: 'doughnut',
                    data: {
                        labels: ['Para el Técnico', 'Comisión Plataforma'],
                        datasets: [{
                            data: [93, 7],
                            backgroundColor: [
                                '#38bdf8', // sky-400
                                '#0369a1'  // sky-700
                            ],
                            borderColor: [
                                '#f3f4f6', // gray-100
                                '#f3f4f6'
                            ],
                            borderWidth: 4
                        }]
                    },
                    options: {
                        responsive: true,
                        maintainAspectRatio: false,
                        plugins: {
                            legend: {
                                position: 'bottom',
                                labels: {
                                    font: {
                                        size: 14,
                                        family: 'Inter'
                                    }
                                }
                            },
                            tooltip: {
                                callbacks: {
                                    label: function(context) {
                                        return `${context.label}: ${context.raw}%`;
                                    }
                                }
                            }
                        },
                        cutout: '60%'
                    }
                });
            }

            buildStepper();
            renderPhase(0);

        });
    </script>
</body>
</html>
