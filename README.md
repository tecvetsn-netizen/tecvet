<!DOCTYPE html>
<html lang="fr" class="">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta name="description" content="Générateur de recettes par IA - Transformez vos ingrédients en délicieuses recettes">
    <meta name="theme-color" content="#4f46e5">
    <link rel="manifest" href="data:application/json;base64,ewogICJuYW1lIjogIkxlIENoZWYgSUEiLAogICJzaG9ydF9uYW1lIjogIkNoZWYgSUEiLAogICJzdGFydF91cmwiICI6ICIuIiwKICAiZGlzcGxheSI6ICJzdGFuZGFsb25lIiwKICAiYmFja2dyb3VuZF9jb2xvciI6ICIjZjFmNWY5IiwKICAidGhlbWVfY29sb3IiOiAiIzRmNDZlNSIsCiAgImljb25zIjogWwogICAgewogICAgICAic3JjIjogImRhdGE6aW1hZ2Uvc3ZnK3htbDtiYXNlNjQsUEhOMlp5QjRiV3h1Y3owaWFIUjBjRG92TDNkM2R5NTNNeTV2Y21jdk1qQXdNQzl6ZG1jaUlIZHBaSFJvUFNJeE1EQWlJSE4wZVd4bFBTSm9kSFJ3T2k4dmQzZDNMbmN6TG05eVp5OHlNREF3TDNOMlp5SStQSEJoZEdnZ1ptbHNiRDBpSTJabVlXWm1ZaUkrUEhCaGRHZ2daRDBpVFRJMU1pSStQR1JsWm5NK0NqeGtaV1p6Q2lBZ0lDQThjbVZqZENCM2FXUjBhRDBpTVRJd01DSWdhR1ZwWjJoMFBTSXhNREFpSUhacFpYZENiM2c5SWpBd01EQXdNREFpTHo0OEwzTjJaejQ9IiwKICAgICAgInNpemVzIjogIjUxMng1MTIiLAogICAgICAidHlwZSI6ICJpbWFnZS9wbmciCiAgICB9CiAgXQp9">
    <title>Le Chef IA - Générateur de Recettes</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        body { 
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #f8fafc 0%, #f1f5f9 100%);
            min-height: 100vh;
        }
        h1, h2, h3, button { 
            font-family: 'Poppins', system-ui, -apple-system, sans-serif;
            font-weight: 700;
        }
        .spinner {
            border: 4px solid rgba(0, 0, 0, 0.1);
            width: 36px;
            height: 36px;
            border-radius: 50%;
            border-left-color: #4f46e5;
            animation: spin 1s ease infinite;
        }
        @keyframes spin { 
            0% { transform: rotate(0deg); } 
            100% { transform: rotate(360deg); } 
        }
        .recipe-card {
            background: linear-gradient(to bottom right, #ffffff, #f9fafb);
            border-radius: 16px;
            box-shadow: 0 10px 25px rgba(0, 0, 0, 0.05);
            overflow: hidden;
            transition: transform 0.3s ease, box-shadow 0.3s ease;
        }
        .recipe-card:hover {
            transform: translateY(-5px);
            box-shadow: 0 15px 30px rgba(0, 0, 0, 0.1);
        }
        .dark .recipe-card {
            background: linear-gradient(to bottom right, #1e293b, #0f172a);
            box-shadow: 0 10px 25px rgba(0, 0, 0, 0.2);
        }
        .ingredient-tag {
            display: inline-block;
            background-color: #e0f2fe;
            color: #0369a1;
            border-radius: 9999px;
            padding: 4px 12px;
            font-size: 0.85rem;
            margin: 2px;
            transition: all 0.2s ease;
        }
        .ingredient-tag:hover {
            transform: translateY(-2px);
            box-shadow: 0 2px 5px rgba(0,0,0,0.1);
        }
        .dark .ingredient-tag {
            background-color: #1e3a8a;
            color: #dbeafe;
        }
        .action-btn {
            transition: all 0.2s ease;
        }
        .action-btn:hover {
            transform: translateY(-2px);
        }
        .bounce {
            animation: bounce 0.5s;
        }
        @keyframes bounce {
            0%, 100% { transform: translateY(0); }
            50% { transform: translateY(-5px); }
        }
        .install-prompt {
            position: fixed;
            bottom: 20px;
            left: 50%;
            transform: translateX(-50%);
            background: white;
            border-radius: 12px;
            padding: 15px;
            box-shadow: 0 10px 25px rgba(0,0,0,0.15);
            display: flex;
            align-items: center;
            gap: 15px;
            z-index: 100;
            animation: slideUp 0.5s ease-out;
            max-width: 90%;
        }
        .dark .install-prompt {
            background: #1e293b;
            color: white;
            border: 1px solid #334155;
        }
        @keyframes slideUp {
            from { bottom: -100px; opacity: 0; }
            to { bottom: 20px; opacity: 1; }
        }
    </style>
</head>
<body class="bg-gray-100 dark:bg-gray-900 text-gray-800 dark:text-gray-200 transition-colors duration-300 min-h-screen">

    <!-- Header -->
    <header class="bg-white dark:bg-gray-800 shadow-md sticky top-0 z-10">
        <div class="container mx-auto px-4 py-3 flex justify-between items-center">
            <div class="flex items-center">
                <div class="bg-indigo-600 w-10 h-10 rounded-lg flex items-center justify-center mr-3">
                    <i class="fas fa-utensils text-white text-xl"></i>
                </div>
                <h1 class="text-xl font-bold text-indigo-600 dark:text-indigo-400">Le Chef IA</h1>
            </div>
            <div class="flex items-center gap-4">
                <button id="theme-toggle" class="p-2 rounded-full hover:bg-gray-200 dark:hover:bg-gray-700">
                    <svg id="theme-icon-dark" class="h-6 w-6 hidden" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M20.354 15.354A9 9 0 018.646 3.646 9.003 9.003 0 0012 21a9.003 9.003 0 008.354-5.646z" />
                    </svg>
                    <svg id="theme-icon-light" class="h-6 w-6 hidden" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 3v1m0 16v1m9-9h-1M4 12H3m15.364 6.364l-.707-.707M6.343 6.343l-.707-.707m12.728 0l-.707.707M6.343 17.657l-.707.707M16 12a4 4 0 11-8 0 4 4 0 018 0z" />
                    </svg>
                </button>
            </div>
        </div>
    </header>

    <!-- Main Content -->
    <main class="container mx-auto p-4 grid grid-cols-1 lg:grid-cols-3 gap-6">
        <!-- Left Column: Controls & Favorites -->
        <div class="lg:col-span-1 space-y-6">
            <!-- Inputs Card -->
            <div class="recipe-card p-5">
                <h2 class="text-xl font-bold mb-4 flex items-center">
                    <i class="fas fa-list-check text-indigo-500 mr-2"></i>
                    Composez votre plat
                </h2>
                <div class="space-y-4">
                    <div>
                        <label class="block text-sm font-medium mb-2">Ingrédients disponibles</label>
                        <div id="ingredients-tags" class="mb-3 flex flex-wrap gap-2">
                            <span class="ingredient-tag">poulet <i class="ml-1 fas fa-times cursor-pointer" onclick="removeIngredient(this)"></i></span>
                            <span class="ingredient-tag">riz <i class="ml-1 fas fa-times cursor-pointer" onclick="removeIngredient(this)"></i></span>
                            <span class="ingredient-tag">tomates <i class="ml-1 fas fa-times cursor-pointer" onclick="removeIngredient(this)"></i></span>
                        </div>
                        <div class="relative">
                            <input 
                                id="ingredient-input" 
                                type="text" 
                                class="w-full p-3 border border-gray-300 dark:border-gray-600 rounded-lg bg-gray-50 dark:bg-gray-700 focus:ring-2 focus:ring-indigo-500" 
                                placeholder="Ajouter un ingrédient (ex: poulet, riz, carottes...)"
                            >
                            <button id="add-ingredient" class="absolute right-2 top-2 bg-indigo-500 hover:bg-indigo-600 text-white rounded-lg px-3 py-1 transition">
                                <i class="fas fa-plus"></i>
                            </button>
                        </div>
                    </div>
                    
                    <div>
                        <label class="block text-sm font-medium mb-2">Type de cuisine</label>
                        <select id="cuisine-select" class="w-full p-3 border border-gray-300 dark:border-gray-600 rounded-lg bg-gray-50 dark:bg-gray-700 focus:ring-2 focus:ring-indigo-500">
                            <option value="">Sélectionnez (optionnel)</option>
                            <option value="Italienne">Italienne</option>
                            <option value="Française">Française</option>
                            <option value="Asiatique" selected>Asiatique</option>
                            <option value="Sénégalaise">Sénégalaise</option>
                            <option value="Mexicaine">Mexicaine</option>
                        </select>
                    </div>
                    
                    <div>
                        <label class="block text-sm font-medium mb-2">Régime alimentaire</label>
                        <select id="regime-select" class="w-full p-3 border border-gray-300 dark:border-gray-600 rounded-lg bg-gray-50 dark:bg-gray-700 focus:ring-2 focus:ring-indigo-500">
                            <option value="">Sélectionnez (optionnel)</option>
                            <option value="Végétarien">Végétarien</option>
                            <option value="Végan">Végan</option>
                            <option value="Sans gluten" selected>Sans gluten</option>
                        </select>
                    </div>
                    
                    <div>
                        <label class="block text-sm font-medium mb-2">Temps de préparation</label>
                        <select id="duree-select" class="w-full p-3 border border-gray-300 dark:border-gray-600 rounded-lg bg-gray-50 dark:bg-gray-700 focus:ring-2 focus:ring-indigo-500">
                            <option value="">Sélectionnez (optionnel)</option>
                            <option value="15 minutes">Moins de 15 min</option>
                            <option value="30 minutes" selected>Moins de 30 min</option>
                            <option value="1 heure">Moins de 1 heure</option>
                        </select>
                    </div>
                    
                    <button id="generate-btn" class="w-full bg-gradient-to-r from-indigo-600 to-purple-600 hover:from-indigo-700 hover:to-purple-700 text-white font-bold py-3 px-4 rounded-lg transition duration-300 ease-in-out flex items-center justify-center text-lg shadow-lg action-btn">
                        <i class="fas fa-magic mr-2"></i>
                        Générer ma recette
                    </button>
                </div>
            </div>
            
            <!-- Favorites Card -->
            <div class="recipe-card p-5">
                <div class="flex justify-between items-center mb-4">
                    <h2 class="text-xl font-bold flex items-center">
                        <i class="fas fa-heart text-red-500 mr-2"></i>
                        Mes Recettes Favorites
                    </h2>
                    <span class="bg-indigo-100 dark:bg-indigo-900 text-indigo-600 dark:text-indigo-300 text-xs font-bold px-2 py-1 rounded-full">
                        <span id="favorites-count">2</span> recettes
                    </span>
                </div>
                <div id="favorites-list" class="space-y-3">
                    <div class="p-3 rounded-lg bg-gray-50 dark:bg-gray-800 hover:bg-gray-100 dark:hover:bg-gray-750 cursor-pointer transition">
                        <div class="font-semibold text-indigo-600 dark:text-indigo-400">Poulet sauté asiatique</div>
                        <div class="text-sm text-gray-500 dark:text-gray-400 mt-1">poulet, poivrons, sauce soja, riz...</div>
                    </div>
                    <div class="p-3 rounded-lg bg-gray-50 dark:bg-gray-800 hover:bg-gray-100 dark:hover:bg-gray-750 cursor-pointer transition">
                        <div class="font-semibold text-indigo-600 dark:text-indigo-400">Risotto aux champignons</div>
                        <div class="text-sm text-gray-500 dark:text-gray-400 mt-1">riz, champignons, parmesan, vin blanc...</div>
                    </div>
                </div>
            </div>
        </div>

        <!-- Right Column: Recipe Display -->
        <div id="recipe-display-area" class="lg:col-span-2">
            <div id="recipe-card" class="recipe-card">
                <div class="p-5 md:p-6">
                    <div class="flex flex-col md:flex-row md:justify-between md:items-start gap-4">
                        <div>
                            <h2 class="text-2xl md:text-3xl font-bold mb-2">Poulet sauté asiatique</h2>
                            <div class="flex flex-wrap gap-2 mt-3">
                                <span class="px-3 py-1 bg-indigo-100 dark:bg-indigo-900 text-indigo-700 dark:text-indigo-300 text-xs font-medium rounded-full">
                                    Asiatique
                                </span>
                                <span class="px-3 py-1 bg-indigo-100 dark:bg-indigo-900 text-indigo-700 dark:text-indigo-300 text-xs font-medium rounded-full">
                                    30 minutes
                                </span>
                                <span class="px-3 py-1 bg-indigo-100 dark:bg-indigo-900 text-indigo-700 dark:text-indigo-300 text-xs font-medium rounded-full">
                                    Sans gluten
                                </span>
                            </div>
                        </div>
                        <div class="flex gap-3">
                            <button class="action-btn p-2 rounded-full bg-red-100 dark:bg-red-900/50 text-red-500 dark:text-red-300">
                                <i class="fas fa-heart text-xl"></i>
                            </button>
                            <button class="action-btn flex items-center text-sm font-medium text-gray-600 dark:text-gray-300 hover:text-indigo-600 dark:hover:text-indigo-400">
                                <i class="fas fa-print"></i>
                            </button>
                        </div>
                    </div>
                    
                    <div class="grid grid-cols-1 md:grid-cols-3 gap-6 mt-6">
                        <div class="md:col-span-1">
                            <h3 class="text-xl font-semibold mb-4 pb-2 border-b border-gray-200 dark:border-gray-700 flex items-center">
                                <i class="fas fa-carrot text-orange-500 mr-2"></i>
                                Ingrédients
                                <span class="text-sm font-normal text-gray-500 ml-2">(8)</span>
                            </h3>
                            <ul class="space-y-3">
                                <li class="flex items-start">
                                    <i class="fas fa-circle text-indigo-500 text-xs mt-2 mr-2"></i>
                                    <span>400g de poulet coupé en dés</span>
                                </li>
                                <li class="flex items-start">
                                    <i class="fas fa-circle text-indigo-500 text-xs mt-2 mr-2"></i>
                                    <span>2 poivrons (rouge et vert)</span>
                                </li>
                                <li class="flex items-start">
                                    <i class="fas fa-circle text-indigo-500 text-xs mt-2 mr-2"></i>
                                    <span>1 oignon</span>
                                </li>
                                <li class="flex items-start">
                                    <i class="fas fa-circle text-indigo-500 text-xs mt-2 mr-2"></i>
                                    <span>2 gousses d'ail</span>
                                </li>
                                <li class="flex items-start">
                                    <i class="fas fa-circle text-indigo-500 text-xs mt-2 mr-2"></i>
                                    <span>3 cuillères à soupe de sauce soja</span>
                                </li>
                                <li class="flex items-start">
                                    <i class="fas fa-circle text-indigo-500 text-xs mt-2 mr-2"></i>
                                    <span>1 cuillère à soupe de miel</span>
                                </li>
                                <li class="flex items-start">
                                    <i class="fas fa-circle text-indigo-500 text-xs mt-2 mr-2"></i>
                                    <span>1 cuillère à café de gingembre râpé</span>
                                </li>
                                <li class="flex items-start">
                                    <i class="fas fa-circle text-indigo-500 text-xs mt-2 mr-2"></i>
                                    <span>Huile de sésame</span>
                                </li>
                            </ul>
                        </div>
                        
                        <div class="md:col-span-2">
                            <h3 class="text-xl font-semibold mb-4 pb-2 border-b border-gray-200 dark:border-gray-700 flex items-center">
                                <i class="fas fa-list-ol text-green-500 mr-2"></i>
                                Préparation
                                <span class="text-sm font-normal text-gray-500 ml-2">(5 étapes)</span>
                            </h3>
                            <ol class="space-y-4">
                                <li class="flex">
                                    <span class="flex-shrink-0 w-8 h-8 rounded-full bg-indigo-100 dark:bg-indigo-900 text-indigo-600 dark:text-indigo-300 flex items-center justify-center font-bold mr-3">
                                        1
                                    </span>
                                    <span>Dans un bol, mélangez la sauce soja, le miel et le gingembre. Ajoutez le poulet et laissez mariner 10 minutes.</span>
                                </li>
                                <li class="flex">
                                    <span class="flex-shrink-0 w-8 h-8 rounded-full bg-indigo-100 dark:bg-indigo-900 text-indigo-600 dark:text-indigo-300 flex items-center justify-center font-bold mr-3">
                                        2
                                    </span>
                                    <span>Faites chauffer l'huile de sésame dans un wok ou une grande poêle. Ajoutez l'oignon et l'ail émincés, faites revenir 2 minutes.</span>
                                </li>
                                <li class="flex">
                                    <span class="flex-shrink-0 w-8 h-8 rounded-full bg-indigo-100 dark:bg-indigo-900 text-indigo-600 dark:text-indigo-300 flex items-center justify-center font-bold mr-3">
                                        3
                                    </span>
                                    <span>Ajoutez le poulet mariné et faites cuire à feu vif pendant 5 minutes jusqu'à ce qu'il soit doré.</span>
                                </li>
                                <li class="flex">
                                    <span class="flex-shrink-0 w-8 h-8 rounded-full bg-indigo-100 dark:bg-indigo-900 text-indigo-600 dark:text-indigo-300 flex items-center justify-center font-bold mr-3">
                                        4
                                    </span>
                                    <span>Incorporez les poivrons coupés en lamelles et faites cuire encore 5 minutes en remuant régulièrement.</span>
                                </li>
                                <li class="flex">
                                    <span class="flex-shrink-0 w-8 h-8 rounded-full bg-indigo-100 dark:bg-indigo-900 text-indigo-600 dark:text-indigo-300 flex items-center justify-center font-bold mr-3">
                                        5
                                    </span>
                                    <span>Servez immédiatement avec du riz blanc, saupoudré de graines de sésame.</span>
                                </li>
                            </ol>
                            
                            <div class="mt-6 p-4 bg-amber-50 dark:bg-amber-900/20 rounded-lg border border-amber-200 dark:border-amber-800">
                                <h4 class="font-bold text-amber-700 dark:text-amber-400 flex items-center">
                                    <i class="fas fa-lightbulb mr-2"></i>
                                    Conseil du Chef IA
                                </h4>
                                <p class="mt-2 text-amber-600 dark:text-amber-300">Pour une version encore plus savoureuse, ajoutez des cacahuètes concassées et un filet de jus de citron vert avant de servir.</p>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </main>
    
    <!-- Install Prompt -->
    <div id="install-prompt" class="install-prompt hidden">
        <div class="flex items-center">
            <div class="bg-indigo-100 dark:bg-indigo-900 w-12 h-12 rounded-lg flex items-center justify-center mr-3">
                <i class="fas fa-download text-indigo-600 dark:text-indigo-400 text-xl"></i>
            </div>
            <div>
                <p class="font-semibold">Installer Le Chef IA</p>
                <p class="text-sm text-gray-600 dark:text-gray-300">Ajoutez à votre écran d'accueil pour une meilleure expérience</p>
            </div>
        </div>
        <button id="install-btn" class="bg-indigo-600 hover:bg-indigo-700 text-white px-4 py-2 rounded-lg font-medium">
            Installer
        </button>
        <button id="cancel-install" class="text-gray-500 hover:text-gray-700 dark:text-gray-400 dark:hover:text-gray-200">
            <i class="fas fa-times"></i>
        </button>
    </div>

    <script>
        document.addEventListener('DOMContentLoaded', () => {
            // --- THEME MANAGEMENT ---
            const themeToggle = document.getElementById('theme-toggle');
            const themeIconDark = document.getElementById('theme-icon-dark');
            const themeIconLight = document.getElementById('theme-icon-light');
            
            const applyTheme = (theme) => {
                if (theme === 'dark') {
                    document.documentElement.classList.add('dark');
                    themeIconLight.classList.remove('hidden');
                    themeIconDark.classList.add('hidden');
                    localStorage.setItem('theme', 'dark');
                } else {
                    document.documentElement.classList.remove('dark');
                    themeIconDark.classList.remove('hidden');
                    themeIconLight.classList.add('hidden');
                    localStorage.setItem('theme', 'light');
                }
            };
            
            themeToggle.addEventListener('click', () => {
                const newTheme = document.documentElement.classList.contains('dark') ? 'light' : 'dark';
                applyTheme(newTheme);
            });

            // Initialize theme
            const savedTheme = localStorage.getItem('theme') || (window.matchMedia('(prefers-color-scheme: dark)').matches ? 'dark' : 'light');
            applyTheme(savedTheme);

            // --- INSTALL PROMPT ---
            let deferredPrompt;
            const installPrompt = document.getElementById('install-prompt');
            const installBtn = document.getElementById('install-btn');
            const cancelInstall = document.getElementById('cancel-install');
            
            // Show install prompt when available
            window.addEventListener('beforeinstallprompt', (e) => {
                e.preventDefault();
                deferredPrompt = e;
                setTimeout(() => {
                    installPrompt.classList.remove('hidden');
                }, 3000);
            });
            
            // Handle install button click
            installBtn.addEventListener('click', async () => {
                if (deferredPrompt) {
                    deferredPrompt.prompt();
                    const { outcome } = await deferredPrompt.userChoice;
                    if (outcome === 'accepted') {
                        installPrompt.classList.add('hidden');
                    }
                    deferredPrompt = null;
                }
            });
            
            // Handle cancel button
            cancelInstall.addEventListener('click', () => {
                installPrompt.classList.add('hidden');
            });

            // --- INGREDIENTS MANAGEMENT ---
            const ingredientInput = document.getElementById('ingredient-input');
            const addIngredientBtn = document.getElementById('add-ingredient');
            const ingredientsTags = document.getElementById('ingredients-tags');
            
            // Function to remove ingredient
            window.removeIngredient = (element) => {
                element.parentElement.remove();
            };
            
            // Add ingredient
            addIngredientBtn.addEventListener('click', () => {
                const ingredient = ingredientInput.value.trim();
                if (ingredient) {
                    const tag = document.createElement('span');
                    tag.className = 'ingredient-tag';
                    tag.innerHTML = `${ingredient} <i class="ml-1 fas fa-times cursor-pointer" onclick="removeIngredient(this)"></i>`;
                    ingredientsTags.appendChild(tag);
                    ingredientInput.value = '';
                }
            });
            
            // Add on Enter key
            ingredientInput.addEventListener('keypress', (e) => {
                if (e.key === 'Enter') {
                    addIngredientBtn.click();
                }
            });

            // --- GENERATE BUTTON ---
            const generateBtn = document.getElementById('generate-btn');
            
            generateBtn.addEventListener('click', () => {
                generateBtn.innerHTML = '<i class="fas fa-spinner animate-spin mr-2"></i> Création en cours...';
                generateBtn.disabled = true;
                
                setTimeout(() => {
                    generateBtn.innerHTML = '<i class="fas fa-magic mr-2"></i> Générer ma recette';
                    generateBtn.disabled = false;
                    
                    // Show success notification
                    const notification = document.createElement('div');
                    notification.className = 'fixed bottom-4 right-4 bg-green-500 text-white px-4 py-2 rounded-lg shadow-lg';
                    notification.innerHTML = '<i class="fas fa-check-circle mr-2"></i> Nouvelle recette générée avec succès !';
                    document.body.appendChild(notification);
                    
                    setTimeout(() => {
                        notification.remove();
                    }, 3000);
                }, 2000);
            });
        });
    </script>
</body>
</html>
