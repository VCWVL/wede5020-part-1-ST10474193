<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Takealot.com Interactive Sitemap</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet">
    <!-- Chosen Palette: Warm Neutrals -->
    <!-- Application Structure Plan: A dashboard layout with a left sidebar for top-level navigation and a main content area for the interactive chart and a contextual information panel. This structure separates controls from the visualization, providing a clear and intuitive user flow. Users can drill down into the sitemap hierarchy by clicking on chart elements or sidebar buttons, preventing information overload and encouraging exploration. -->
    <!-- Visualization & Content Choices: A dynamic horizontal bar chart (Chart.js/Canvas) is used to represent the sitemap hierarchy. Goal: Organize/Inform. Interaction: Users click on bars to drill down into sub-categories. The chart is updated dynamically with JS. Justification: This method avoids overwhelming the user with the full sitemap at once and creates an engaging, step-by-step exploration experience. A separate HTML info panel provides detailed descriptions on-click, keeping the chart clean. NO SVG/Mermaid used. -->
    <!-- CONFIRMATION: NO SVG graphics used. NO Mermaid JS used. -->
    <style>
        body {
            font-family: 'Inter', sans-serif;
            background-color: #fdfdfc;
            color: #333;
        }
        .chart-container {
            position: relative;
            width: 100%;
            max-width: 900px;
            margin-left: auto;
            margin-right: auto;
            height: 50vh;
            max-height: 500px;
        }
        @media (max-width: 768px) {
            .chart-container {
                height: 60vh;
            }
        }
        .nav-button {
            transition: all 0.3s ease;
        }
        .nav-button.active {
            background-color: #0B4F6C;
            color: #FFFFFF;
            font-weight: 600;
        }
        .info-panel {
            transition: background-color 0.3s ease;
        }
    </style>
</head>
<body class="antialiased">

    <div class="min-h-screen flex flex-col">
        <header class="bg-white shadow-sm p-4">
            <h1 class="text-2xl md:text-3xl font-bold text-center text-[#0B4F6C]">Takealot.com Interactive Sitemap</h1>
            <p class="text-center text-gray-500 mt-1">Explore the structure of South Africa's leading online store.</p>
        </header>

        <div class="flex-grow flex flex-col md:flex-row p-4 md:p-6 gap-6">
            <nav class="w-full md:w-1/4 lg:w-1/5 bg-white p-4 rounded-lg shadow-md">
                <h2 class="text-lg font-semibold mb-4 border-b pb-2">Main Sections</h2>
                <div id="navigation" class="space-y-2">
                    <!-- Navigation buttons will be injected here -->
                </div>
                 <button id="backButton" class="w-full mt-6 bg-gray-300 text-gray-800 py-2 px-4 rounded-lg hover:bg-gray-400 focus:outline-none focus:ring-2 focus:ring-gray-500 disabled:opacity-50 disabled:cursor-not-allowed" disabled>
                    &larr; Back
                </button>
            </nav>

            <main class="w-full md:w-3/4 lg:w-4/5 flex flex-col gap-6">
                <div class="bg-white p-4 md:p-6 rounded-lg shadow-md flex-grow">
                     <h3 id="chartTitle" class="text-xl font-semibold text-center mb-4">Website Structure Overview</h3>
                    <div class="chart-container">
                        <canvas id="sitemapChart"></canvas>
                    </div>
                </div>
                <div id="infoPanel" class="bg-white p-6 rounded-lg shadow-md info-panel min-h-[150px]">
                    <h3 id="infoTitle" class="text-lg font-bold text-[#0B4F6C]">Welcome!</h3>
                    <p id="infoContent" class="text-gray-600 mt-2">Click on a section in the navigation menu or a bar in the chart to begin exploring the Takealot.com sitemap. This panel will provide more details about the selected section.</p>
                </div>
            </main>
        </div>
    </div>

    <script>
        const sitemapData = {
            title: "Takealot.com",
            children: [
                {
                    title: "Main Pages",
                    description: "Core pages that form the primary user entry points and showcase dynamic content like deals and featured products.",
                    children: [
                        { title: "Homepage", description: "The main landing page featuring daily deals, promotions, and trending items." },
                        { title: "Daily Deals", description: "A dedicated section for time-sensitive promotions and discounts." },
                        { title: "Featured Products", description: "Curated collections of popular or new products." },
                        { title: "New Arrivals", description: "Showcases the latest products added to the store." }
                    ]
                },
                {
                    title: "Shop by Category",
                    description: "The primary product hierarchy, allowing users to browse through various departments to find items.",
                    children: [
                        {
                            title: "Electronics",
                            description: "Category for all electronic devices and gadgets.",
                            children: [
                                { title: "Computers & Laptops", description: "Desktops, laptops, and accessories." },
                                { title: "Cellphones & Wearables", description: "Smartphones, smartwatches, and related tech." },
                                { title: "TVs & Audio", description: "Televisions, sound systems, and home entertainment." },
                                { title: "Gaming", description: "Consoles, games, and gaming accessories." },
                                { title: "Photography", description: "Cameras, lenses, and photo equipment." }
                            ]
                        },
                        {
                            title: "Lifestyle",
                            description: "A broad category covering home, personal care, and recreational products.",
                             children: [
                                { title: "Home & Kitchen", description: "Appliances, decor, and kitchenware." },
                                { title: "Health & Beauty", description: "Personal care, cosmetics, and wellness products." },
                                { title: "Sports & Outdoors", description: "Sporting goods and outdoor equipment." },
                                { title: "Toys & Baby", description: "Products for children, from toys to baby care essentials." },
                                { title: "Automotive", description: "Car parts, accessories, and maintenance products." }
                            ]
                        },
                        { title: "Fashion", description: "Clothing, shoes, and accessories for all ages." },
                        { title: "Books & Media", description: "Physical and digital media including books, movies, and music." },
                        { title: "Groceries & Pantry", description: "Everyday essentials, food items, and household supplies." }
                    ]
                },
                {
                    title: "User & Account",
                    description: "Pages dedicated to user account management, including order history, personal details, and settings.",
                    children: [
                        { title: "My Account", description: "The central dashboard for all user-related information." },
                        { title: "Order History", description: "A list of all past and current orders." },
                        { title: "Wishlist", description: "A collection of saved items for future purchase." },
                        { title: "Profile & Details", description: "Manage personal information and login credentials." },
                        { title: "Address Book", description: "Save and manage shipping addresses." },
                        { title: "Payment Methods", description: "Manage saved credit cards and other payment options." },
                        { title: "Returns", description: "Initiate and track product returns." },
                        { title: "Login/Register", description: "Page for user authentication and new account creation." }
                    ]
                },
                {
                    title: "Shopping & Checkout",
                    description: "The functional user journey from finding a product to completing a purchase.",
                    children: [
                        { title: "Search Results", description: "Displays products matching a user's search query." },
                        { title: "Product Detail Page", description: "Shows detailed information about a single product." },
                        { title: "Shopping Cart", description: "A temporary collection of items selected for purchase." },
                        { title: "Checkout Process", description: "The multi-step process for payment and shipping confirmation." }
                    ]
                },
                {
                    title: "Services & Information",
                    description: "Informational and support pages that provide help, company background, and legal notices.",
                    children: [
                        { title: "Customer Help Center", description: "A portal for support, FAQs, and contacting customer service." },
                        { title: "Seller Portal", description: "Information and login for third-party sellers." },
                        { title: "About Us", description: "Company history, mission, and career information." },
                        { title: "Privacy Policy", description: "Details on how user data is collected and used." },
                        { title: "Terms & Conditions", description: "The legal terms governing the use of the website." },
                        { title: "Delivery & Shipping", description: "Information on delivery options, costs, and timelines." }
                    ]
                }
            ]
        };

        const ctx = document.getElementById('sitemapChart').getContext('2d');
        let sitemapChart;
        const navigationHistory = [];

        const backButton = document.getElementById('backButton');
        const navigationContainer = document.getElementById('navigation');
        const chartTitleEl = document.getElementById('chartTitle');
        const infoTitleEl = document.getElementById('infoTitle');
        const infoContentEl = document.getElementById('infoContent');

        function updateInfoPanel(title, content) {
            infoTitleEl.textContent = title;
            infoContentEl.textContent = content;
        }

        function renderNavigation(data) {
            navigationContainer.innerHTML = '';
            data.children.forEach(item => {
                const button = document.createElement('button');
                button.textContent = item.title;
                button.className = 'w-full text-left py-2 px-4 rounded-lg hover:bg-gray-100 focus:outline-none focus:ring-2 focus:ring-[#0B4F6C] nav-button';
                button.onclick = () => {
                    if (item.children && item.children.length > 0) {
                        navigationHistory.push(getCurrentData());
                        renderChart(item);
                        backButton.disabled = false;
                    }
                    updateInfoPanel(item.title, item.description);
                };
                navigationContainer.appendChild(button);
            });
        }
        
        function getCurrentData() {
            if (navigationHistory.length === 0) {
                return sitemapData;
            }
            return navigationHistory[navigationHistory.length - 1];
        }

        function renderChart(data) {
            const labels = data.children.map(item => item.title);
            const dataPoints = data.children.map(item => (item.children ? item.children.length : 1));
            const hasChildren = data.children.map(item => (item.children && item.children.length > 0));

            chartTitleEl.textContent = data.title === "Takealot.com" ? "Website Structure Overview" : `Section: ${data.title}`;
            updateNavButtons(data.title);

            if (sitemapChart) {
                sitemapChart.destroy();
            }

            sitemapChart = new Chart(ctx, {
                type: 'bar',
                data: {
                    labels: labels,
                    datasets: [{
                        label: 'Number of Sub-pages',
                        data: dataPoints,
                        backgroundColor: hasChildren.map(hc => hc ? 'rgba(11, 79, 108, 0.7)' : 'rgba(255, 199, 44, 0.7)'),
                        borderColor: hasChildren.map(hc => hc ? 'rgba(11, 79, 108, 1)' : 'rgba(255, 199, 44, 1)'),
                        borderWidth: 1,
                        hoverBackgroundColor: hasChildren.map(hc => hc ? 'rgba(11, 79, 108, 1)' : 'rgba(255, 199, 44, 1)'),
                    }]
                },
                options: {
                    indexAxis: 'y',
                    responsive: true,
                    maintainAspectRatio: false,
                    scales: {
                        x: {
                            beginAtZero: true,
                            title: {
                                display: true,
                                text: 'Number of Sub-pages'
                            }
                        },
                        y: {
                           ticks: {
                                autoSkip: false,
                                callback: function(value, index, values) {
                                    const label = this.getLabelForValue(value);
                                    return label.length > 25 ? label.substring(0, 22) + '...' : label;
                                }
                            }
                        }
                    },
                    plugins: {
                        legend: {
                            display: false
                        },
                        tooltip: {
                            callbacks: {
                                label: function(context) {
                                    let label = context.dataset.label || '';
                                    if (label) {
                                        label += ': ';
                                    }
                                    if (context.parsed.x !== null) {
                                        const node = data.children[context.dataIndex];
                                        label += `${context.parsed.x} | Click to ${node.children ? 'explore' : 'view info'}.`;
                                    }
                                    return label;
                                }
                            }
                        }
                    },
                    onClick: (event, elements) => {
                        if (elements.length > 0) {
                            const index = elements[0].index;
                            const selectedNode = data.children[index];
                            updateInfoPanel(selectedNode.title, selectedNode.description);

                            if (selectedNode.children && selectedNode.children.length > 0) {
                                navigationHistory.push(data);
                                renderChart(selectedNode);
                                backButton.disabled = false;
                            }
                        }
                    }
                }
            });
        }
        
        function updateNavButtons(activeTitle) {
            const buttons = document.querySelectorAll('.nav-button');
            buttons.forEach(button => {
                if (button.textContent === activeTitle) {
                    button.classList.add('active');
                } else {
                    button.classList.remove('active');
                }
            });
        }

        backButton.addEventListener('click', () => {
            if (navigationHistory.length > 0) {
                const previousData = navigationHistory.pop();
                renderChart(previousData);
                updateInfoPanel(previousData.title, previousData.description || "Navigated back to the parent section.");
            }
            if (navigationHistory.length === 0) {
                backButton.disabled = true;
            }
        });

        // Initial Load
        renderNavigation(sitemapData);
        renderChart(sitemapData);
    </script>
</body>
</html>
