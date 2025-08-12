<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Interactive CV - Jasmin Jimenez</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;700&display=swap" rel="stylesheet">
    <!-- Chosen Palette: Warm Neutrals -->
    <!-- Application Structure Plan: The application uses a single-page, vertical-scroll structure designed for intuitive exploration by recruiters. It starts with a prominent header and professional summary. This is followed by an interactive skills dashboard where hovering over a skill category highlights relevant roles in the career timeline below. The core of the app is this interactive timeline, where clicking a job reveals its full details. This master-detail approach keeps the interface clean. A "Key Expertise" chart provides a quick visual summary of experience distribution, a high-impact feature for scannability. This structure was chosen to transform the static CV into an engaging narrative, allowing users to quickly grasp the breadth of experience and then dive into specifics as needed, which is more effective than a linear text document. -->
    <!-- Visualization & Content Choices: 
        - Professional Summary: Report Info -> Goal: Inform -> Viz: Formatted Text -> Interaction: None -> Justification: Clear, immediate introduction.
        - Key Skills: Report Info -> Goal: Organize/Relate -> Viz: Interactive HTML Tags -> Interaction: Hover -> Justification: Connects skills to specific jobs, providing context. Method: JS.
        - Career Timeline: Report Info -> Goal: Explore Change -> Viz: HTML/CSS Timeline + Detail Pane -> Interaction: Click -> Justification: Breaks down a long work history into digestible, user-selected chunks. Method: JS.
        - Expertise Distribution: Synthesized Data -> Goal: Compare -> Viz: Bar Chart -> Interaction: Hover Tooltip -> Justification: Provides a high-level, quantitative summary of experience areas for quick assessment. Library: Chart.js.
        - Education: Report Info -> Goal: Inform -> Viz: Formatted Text -> Interaction: None -> Justification: Standard, clear presentation of qualifications.
    -->
    <!-- CONFIRMATION: NO SVG graphics used. NO Mermaid JS used. -->
    <style>
        body {
            font-family: 'Inter', sans-serif;
            background-color: #FDFBF8;
            color: #4A4A4A;
        }
        .timeline-item::before {
            content: '';
            position: absolute;
            left: -2.75rem;
            top: 0.5rem;
            width: 1rem;
            height: 1rem;
            border-radius: 50%;
            background-color: #D4C4B3;
            border: 3px solid #FDFBF8;
        }
        .timeline-item.active::before {
            background-color: #A88B64;
        }
        .skill-tag {
            transition: all 0.3s ease;
        }
        .timeline-item.highlighted {
            background-color: #F4ECE0;
            border-color: #A88B64;
        }
        .chart-container {
            position: relative;
            width: 100%;
            max-width: 800px;
            margin-left: auto;
            margin-right: auto;
            height: 350px;
            max-height: 400px;
        }
        @media (min-width: 768px) {
            .chart-container {
                height: 400px;
            }
        }
    </style>
</head>
<body class="antialiased">

    <div class="container mx-auto p-4 sm:p-8 max-w-6xl">

        <!-- Header Section -->
        <header class="text-center mb-12 border-b border-gray-200 pb-8">
            <h1 class="text-4xl md:text-5xl font-bold text-gray-800">Jasmin Jimenez</h1>
            <p class="text-xl text-gray-600 mt-2">Experienced and Adaptable Administrative Professional</p>
            <div class="flex justify-center items-center space-x-4 mt-4 text-gray-500">
                <span>&#128205; Sheffield, UK</span>
                <span>&#9993; jdjimenez.work@gmail.com</span>
                <span>&#128222; +44-787-950-8706</span>
            </div>
             <a href="https://www.linkedin.com/in/jasmin-jimenez-36b91834" target="_blank" class="inline-block mt-4 text-[#A88B64] hover:underline">LinkedIn Profile</a>
        </header>

        <main>
            <!-- Professional Summary Section -->
            <section id="summary" class="mb-16">
                 <h2 class="text-2xl font-bold text-center mb-4 text-gray-700">Professional Summary</h2>
                 <p class="text-center max-w-3xl mx-auto text-gray-600">
                    This section provides a high-level overview of my professional background and career goals. It summarizes over a decade of experience in various support roles, highlighting my adaptability and commitment.
                </p>
                <div class="mt-6 p-6 bg-white rounded-lg shadow-sm">
                    <p class="text-lg text-gray-700 leading-relaxed">A highly efficient and organized professional with over a decade of diverse experience in executive assistance, virtual support, logistics coordination, and social media management. Possessing a solid foundation of organizational excellence and a proven ability to adapt quickly to new environments and workflows. Seeking to leverage a strong background in customer service and administrative support to contribute to a UK-based organization that values commitment, professionalism, and initiative.</p>
                </div>
            </section>

            <!-- Skills Section -->
            <section id="skills" class="mb-16">
                <h2 class="text-2xl font-bold text-center mb-4 text-gray-700">Core Competencies</h2>
                <p class="text-center max-w-3xl mx-auto text-gray-600 mb-8">
                    Here you can explore my key skill sets. Hover over any skill category to see the related job roles in the Career Timeline below highlighted. This demonstrates the practical application of my abilities throughout my career.
                </p>
                <div id="skills-container" class="flex flex-wrap justify-center gap-4">
                    <!-- Skills will be dynamically inserted here -->
                </div>
            </section>

            <!-- Experience Section -->
            <section id="experience" class="mb-16">
                <h2 class="text-2xl font-bold text-center mb-4 text-gray-700">Career Timeline</h2>
                 <p class="text-center max-w-3xl mx-auto text-gray-600 mb-12">
                    Below is my professional journey. Click on any role in the timeline on the left to view the detailed responsibilities and achievements in the panel on the right. The currently selected role is highlighted for clarity.
                </p>
                <div class="grid md:grid-cols-2 gap-12">
                    <!-- Timeline -->
                    <div class="relative">
                        <div class="border-l-2 border-gray-200 absolute h-full left-[-2.25rem] top-2"></div>
                        <div id="timeline-container" class="space-y-8">
                            <!-- Timeline items will be dynamically inserted here -->
                        </div>
                    </div>
                    <!-- Job Details -->
                    <div id="details-container" class="bg-white p-6 rounded-lg shadow-sm sticky top-8 h-fit">
                        <h3 id="details-title" class="text-xl font-bold text-gray-800 mb-2">Select a role to see details</h3>
                        <p id="details-company" class="text-md text-gray-500 mb-4"></p>
                        <ul id="details-list" class="list-disc list-inside text-gray-600 space-y-2">
                           <li>Details about the selected role will appear here.</li>
                        </ul>
                    </div>
                </div>
            </section>
            
            <!-- Expertise Distribution Chart Section -->
            <section id="chart-section" class="mb-16">
                <h2 class="text-2xl font-bold text-center mb-4 text-gray-700">Expertise Distribution</h2>
                <p class="text-center max-w-3xl mx-auto text-gray-600 mb-8">
                    This chart visually represents the distribution of my experience across key functional areas. It provides a quick overview of my primary strengths, based on the time spent in roles with significant responsibilities in each domain.
                </p>
                <div class="bg-white p-4 sm:p-6 rounded-lg shadow-sm">
                    <div class="chart-container">
                        <canvas id="experienceChart"></canvas>
                    </div>
                </div>
            </section>

            <!-- Education Section -->
            <section id="education">
                 <h2 class="text-2xl font-bold text-center mb-4 text-gray-700">Education</h2>
                 <p class="text-center max-w-3xl mx-auto text-gray-600 mb-8">
                    My formal academic qualification is listed below, which provided the foundational business knowledge for my career.
                </p>
                <div class="bg-white p-6 rounded-lg shadow-sm text-center">
                    <h3 class="text-lg font-bold text-gray-800">Bachelor of Science in Business Administration</h3>
                    <p class="text-md text-gray-600">Major in Business Management</p>
                    <p class="text-md text-gray-500 mt-1">Holy Angel University, Pampanga, Philippines (2003 - 2007)</p>
                </div>
            </section>
        </main>

    </div>

    <script>
        const cvData = {
            skills: [
                { name: 'Administration & Executive Support', tags: ['admin', 'exec-support'], details: ['Diary Management', 'Expense Reporting', 'Travel & Logistics Coordination', 'Meeting Organisation', 'Stakeholder Communication'] },
                { name: 'Customer Service & Coordination', tags: ['customer-service', 'coordination'], details: ['Client Relationship Management', 'Order Fulfilment', 'Problem Resolution', 'Supplier Liaison'] },
                { name: 'Marketing & Social Media', tags: ['marketing', 'social-media'], details: ['Content Management', 'Social Media Strategy', 'Reporting & Analytics', 'Basic Graphic Design', 'HTML'] },
                { name: 'Logistics', tags: ['logistics'], details: ['Inventory Management', 'Shipping Coordination', 'Supply Chain Resolution', 'Documentation'] },
                { name: 'Software Proficiency', tags: ['software'], details: ['Microsoft Office Suite', 'SAP', 'Asana', 'Google Analytics', 'Various social media platforms'] }
            ],
            experience: [
                {
                    id: 1,
                    title: 'Associate Logistics Specialist',
                    company: 'Algaecal',
                    location: 'Remote (from Sheffield, UK)',
                    period: '2024 - 2025',
                    description: [
                        'Coordinated and oversaw daily logistics operations, including inventory management, shipping, and order fulfilment.',
                        'Liaised with suppliers, freight carriers, and warehouse teams to ensure timely and cost-effective delivery of products.',
                        'Monitored and tracked shipments, proactively resolving delivery and supply chain issues.',
                        'Maintained accurate records of shipments, invoices, and logistics documentation.'
                    ],
                    skillTags: ['logistics', 'coordination', 'software']
                },
                {
                    id: 2,
                    title: 'Virtual Assistant',
                    company: 'Doxa7 Services',
                    location: 'Remote',
                    period: '2022 - 2023',
                    description: [
                        'Managed the preparation of reports, presentations, and other documents for clients.',
                        'Monitored project deadlines and provided progress summaries to stakeholders.',
                        'Prepared and managed expense reports and invoices.',
                        'Monitored and updated social media accounts, ensuring consistent brand presence.'
                    ],
                    skillTags: ['admin', 'exec-support', 'social-media', 'software']
                },
                {
                    id: 3,
                    title: 'Marketing/Sales Assistant',
                    company: 'Embark/Maker\'s Row',
                    location: 'Remote',
                    period: '2021 - 2022',
                    description: [
                        'Undertook daily administrative tasks to support the marketing and sales teams.',
                        'Assisted marketing executives in the organisation and execution of various projects.',
                        'Managed social media accounts, reported on social media metrics, and utilised Google Analytics to inform strategy.'
                    ],
                    skillTags: ['marketing', 'social-media', 'admin', 'software']
                },
                {
                    id: 4,
                    title: 'Virtual Assistant',
                    company: 'Can We Talk Pty Ltd',
                    location: 'Remote',
                    period: '2019 - 2021',
                    description: [
                        'Managed LinkedIn and other social media platforms.',
                        'Uploaded blogs and other content to company websites and social media channels.',
                        'Provided comprehensive administrative support to the CEO\'s Business Manager, including travel and hotel bookings.'
                    ],
                    skillTags: ['admin', 'exec-support', 'social-media', 'marketing']
                },
                {
                    id: 5,
                    title: 'DevOps Assistant',
                    company: 'MTG Media Group',
                    location: 'Remote',
                    period: '2018 - 2022',
                    description: [
                        'Proofread content for various company magazine platforms.',
                        'Provided support for the company\'s ticketing system.',
                        'Managed and distributed monthly reports.',
                        'Uploaded and formatted content for clients, including basic HTML content creation.'
                    ],
                    skillTags: ['admin', 'software', 'marketing']
                },
                {
                    id: 6,
                    title: 'Executive Services Specialist',
                    company: 'Cloudstaff Inc.',
                    location: 'Remote',
                    period: '2014 - 2018',
                    description: [
                        'Provided high-level administrative support, including email and calendar management.',
                        'Managed file organisation and client communication.',
                        'Tracked and lodged loan proposals and payments, ensuring all data was accurately entered and up to date.'
                    ],
                    skillTags: ['exec-support', 'admin', 'customer-service']
                },
                {
                    id: 7,
                    title: 'Executive Secretary/Administrative Assistant',
                    company: 'Viskase Asia Pacific Corporation',
                    location: 'Manila, Philippines',
                    period: '2011 - 2014',
                    description: [
                        'Prepared agendas and made arrangements for management meetings.',
                        'Managed travel arrangements and processed visa applications for executives.',
                        'Processed purchase requisitions through SAP to ensure office supplies were readily available.',
                        'Spearheaded employee and company activities.'
                    ],
                    skillTags: ['exec-support', 'admin', 'coordination', 'software']
                }
            ]
        };

        document.addEventListener('DOMContentLoaded', () => {
            const skillsContainer = document.getElementById('skills-container');
            const timelineContainer = document.getElementById('timeline-container');
            const detailsTitle = document.getElementById('details-title');
            const detailsCompany = document.getElementById('details-company');
            const detailsList = document.getElementById('details-list');

            // Populate Skills
            cvData.skills.forEach(skill => {
                const skillEl = document.createElement('div');
                skillEl.className = 'skill-tag cursor-pointer bg-white text-gray-700 py-2 px-4 rounded-full shadow-sm hover:bg-[#A88B64] hover:text-white hover:shadow-md';
                skillEl.textContent = skill.name;
                skillEl.dataset.skillTags = JSON.stringify(skill.tags);
                skillsContainer.appendChild(skillEl);

                skillEl.addEventListener('mouseover', () => {
                    const tags = JSON.parse(skillEl.dataset.skillTags);
                    document.querySelectorAll('.timeline-item').forEach(item => {
                        const itemTags = JSON.parse(item.dataset.skillTags);
                        if (tags.some(tag => itemTags.includes(tag))) {
                            item.classList.add('highlighted');
                        }
                    });
                });

                skillEl.addEventListener('mouseout', () => {
                    document.querySelectorAll('.timeline-item').forEach(item => {
                        item.classList.remove('highlighted');
                    });
                });
            });

            // Populate Timeline
            cvData.experience.forEach((job, index) => {
                const jobEl = document.createElement('div');
                jobEl.className = 'timeline-item relative pl-8 pb-8 cursor-pointer border border-transparent rounded-lg p-4 transition-all duration-300';
                jobEl.dataset.id = job.id;
                jobEl.dataset.skillTags = JSON.stringify(job.skillTags);

                jobEl.innerHTML = `
                    <h3 class="text-lg font-bold text-gray-800">${job.title}</h3>
                    <p class="text-md text-gray-600">${job.company}</p>
                    <p class="text-sm text-gray-400">${job.period}</p>
                `;
                timelineContainer.appendChild(jobEl);

                jobEl.addEventListener('click', () => {
                    const selectedJob = cvData.experience.find(j => j.id == job.id);
                    detailsTitle.textContent = selectedJob.title;
                    detailsCompany.innerHTML = `${selectedJob.company} &bull; ${selectedJob.period}`;
                    detailsList.innerHTML = '';
                    selectedJob.description.forEach(desc => {
                        const li = document.createElement('li');
                        li.textContent = desc;
                        detailsList.appendChild(li);
                    });

                    document.querySelectorAll('.timeline-item').forEach(item => item.classList.remove('active'));
                    jobEl.classList.add('active');
                });

                if (index === 0) {
                    jobEl.click();
                }
            });
            
            // Chart.js Implementation
            const ctx = document.getElementById('experienceChart').getContext('2d');
            const experienceData = {
                'Admin & Exec Support': 8,
                'Logistics & Coordination': 4,
                'Marketing & Social Media': 5,
                'Customer Service': 4,
            };

            const chart = new Chart(ctx, {
                type: 'bar',
                data: {
                    labels: Object.keys(experienceData),
                    datasets: [{
                        label: 'Years of Experience',
                        data: Object.values(experienceData),
                        backgroundColor: '#D4C4B3',
                        borderColor: '#A88B64',
                        borderWidth: 1,
                        borderRadius: 4,
                        hoverBackgroundColor: '#A88B64'
                    }]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    indexAxis: 'y',
                    scales: {
                        x: {
                            beginAtZero: true,
                            grid: {
                                display: false
                            },
                            ticks: {
                                font: {
                                    family: "'Inter', sans-serif"
                                }
                            }
                        },
                        y: {
                             grid: {
                                display: false
                            },
                            ticks: {
                                font: {
                                    family: "'Inter', sans-serif"
                                }
                            }
                        }
                    },
                    plugins: {
                        legend: {
                            display: false
                        },
                        tooltip: {
                            enabled: true,
                            backgroundColor: '#4A4A4A',
                            titleFont: {
                                family: "'Inter', sans-serif",
                                size: 14,
                                weight: 'bold'
                            },
                            bodyFont: {
                                family: "'Inter', sans-serif",
                                size: 12
                            },
                            callbacks: {
                                label: function(context) {
                                    let label = context.dataset.label || '';
                                    if (label) {
                                        label += ': ';
                                    }
                                    if (context.parsed.x !== null) {
                                        label += context.parsed.x + ' years';
                                    }
                                    return label;
                                }
                            }
                        }
                    }
                }
            });
        });
    </script>
</body>
</html>
