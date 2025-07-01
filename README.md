# ai-powered-resume-parser
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>AI Resume Parser</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
            padding: 20px;
        }

        .container {
            max-width: 1200px;
            margin: 0 auto;
            background: white;
            border-radius: 20px;
            box-shadow: 0 20px 40px rgba(0,0,0,0.1);
            overflow: hidden;
        }

        .header {
            background: linear-gradient(135deg, #4facfe 0%, #00f2fe 100%);
            padding: 40px;
            text-align: center;
            color: white;
        }

        .header h1 {
            font-size: 2.5em;
            margin-bottom: 10px;
            text-shadow: 0 2px 4px rgba(0,0,0,0.3);
        }

        .header p {
            font-size: 1.2em;
            opacity: 0.9;
        }

        .main-content {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 40px;
            padding: 40px;
        }

        .upload-section {
            background: #f8f9ff;
            border-radius: 15px;
            padding: 30px;
            border: 2px dashed #4facfe;
            text-align: center;
            transition: all 0.3s ease;
        }

        .upload-section:hover {
            border-color: #667eea;
            background: #f0f4ff;
            transform: translateY(-2px);
        }

        .upload-zone {
            padding: 40px 20px;
            cursor: pointer;
        }

        .upload-icon {
            font-size: 3em;
            color: #4facfe;
            margin-bottom: 20px;
        }

        .upload-text {
            font-size: 1.1em;
            color: #555;
            margin-bottom: 20px;
        }

        .file-input {
            display: none;
        }

        .upload-btn {
            background: linear-gradient(135deg, #4facfe 0%, #00f2fe 100%);
            color: white;
            border: none;
            padding: 12px 30px;
            border-radius: 25px;
            cursor: pointer;
            font-size: 1em;
            font-weight: 600;
            transition: all 0.3s ease;
        }

        .upload-btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 5px 15px rgba(79, 172, 254, 0.4);
        }

        .text-input {
            margin-top: 20px;
        }

        .text-input textarea {
            width: 100%;
            height: 200px;
            padding: 15px;
            border: 2px solid #e1e5e9;
            border-radius: 10px;
            font-size: 14px;
            resize: vertical;
            transition: border-color 0.3s ease;
        }

        .text-input textarea:focus {
            outline: none;
            border-color: #4facfe;
        }

        .parse-btn {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
            border: none;
            padding: 15px 40px;
            border-radius: 25px;
            cursor: pointer;
            font-size: 1.1em;
            font-weight: 600;
            margin-top: 20px;
            width: 100%;
            transition: all 0.3s ease;
        }

        .parse-btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 5px 15px rgba(102, 126, 234, 0.4);
        }

        .parse-btn:disabled {
            opacity: 0.6;
            cursor: not-allowed;
            transform: none;
            box-shadow: none;
        }

        .results-section {
            background: #fff;
            border-radius: 15px;
            padding: 30px;
            border: 1px solid #e1e5e9;
        }

        .results-title {
            font-size: 1.5em;
            color: #333;
            margin-bottom: 25px;
            display: flex;
            align-items: center;
            gap: 10px;
        }

        .field-group {
            margin-bottom: 25px;
            opacity: 0;
            transform: translateY(20px);
            animation: fadeInUp 0.5s ease forwards;
        }

        .field-group:nth-child(2) { animation-delay: 0.1s; }
        .field-group:nth-child(3) { animation-delay: 0.2s; }
        .field-group:nth-child(4) { animation-delay: 0.3s; }
        .field-group:nth-child(5) { animation-delay: 0.4s; }
        .field-group:nth-child(6) { animation-delay: 0.5s; }

        @keyframes fadeInUp {
            to {
                opacity: 1;
                transform: translateY(0);
            }
        }

        .field-label {
            font-weight: 600;
            color: #4facfe;
            margin-bottom: 8px;
            font-size: 0.9em;
            text-transform: uppercase;
            letter-spacing: 0.5px;
        }

        .field-value {
            background: #f8f9ff;
            padding: 15px;
            border-radius: 8px;
            border-left: 4px solid #4facfe;
            color: #333;
            line-height: 1.6;
        }

        .skills-list {
            display: flex;
            flex-wrap: wrap;
            gap: 8px;
            margin-top: 10px;
        }

        .skill-tag {
            background: linear-gradient(135deg, #4facfe 0%, #00f2fe 100%);
            color: white;
            padding: 6px 12px;
            border-radius: 20px;
            font-size: 0.85em;
            font-weight: 500;
        }

        .loading {
            display: none;
            text-align: center;
            padding: 40px;
        }

        .loading.show {
            display: block;
        }

        .spinner {
            width: 50px;
            height: 50px;
            border: 4px solid #f3f3f3;
            border-top: 4px solid #4facfe;
            border-radius: 50%;
            animation: spin 1s linear infinite;
            margin: 0 auto 20px;
        }

        @keyframes spin {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }

        .error {
            background: #ffe6e6;
            color: #d63031;
            padding: 15px;
            border-radius: 8px;
            border-left: 4px solid #d63031;
            margin-top: 20px;
        }

        .score-circle {
            width: 60px;
            height: 60px;
            border-radius: 50%;
            background: conic-gradient(#4facfe 0deg, #4facfe calc(var(--score) * 3.6deg), #e1e5e9 calc(var(--score) * 3.6deg));
            display: flex;
            align-items: center;
            justify-content: center;
            font-weight: 700;
            font-size: 1.2em;
            color: #333;
            position: relative;
        }

        .score-circle::before {
            content: '';
            position: absolute;
            width: 45px;
            height: 45px;
            background: white;
            border-radius: 50%;
            z-index: -1;
        }

        .keyword-match {
            background: linear-gradient(135deg, #a8e6cf 0%, #88d8a3 100%);
            color: #2d5016;
            padding: 4px 8px;
            border-radius: 12px;
            font-size: 0.8em;
            font-weight: 600;
        }

        .section-divider {
            height: 2px;
            background: linear-gradient(135deg, #4facfe 0%, #00f2fe 100%);
            margin: 30px 0;
            border-radius: 1px;
        }

        .recommendation {
            background: linear-gradient(135deg, #ffeaa7 0%, #fdcb6e 100%);
            padding: 20px;
            border-radius: 10px;
            margin-top: 20px;
            border-left: 4px solid #e17055;
        }

        .recommendation h4 {
            color: #2d3436;
            margin-bottom: 10px;
            font-size: 1.1em;
        }

        .recommendation p {
            color: #636e72;
            margin: 0;
            line-height: 1.5;
        }
            .main-content {
                grid-template-columns: 1fr;
                gap: 20px;
                padding: 20px;
            }
            
            .header {
                padding: 30px 20px;
            }
            
            .header h1 {
                font-size: 2em;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="header">
            <h1>📋 Resume Parser</h1>
            <p>Develop an AI system to extract and organize information from resumes, enhancing HR processes and candidate selection.</p>
        </div>
        
        <div class="main-content">
            <div class="upload-section">
                <div class="upload-zone" onclick="document.getElementById('fileInput').click()">
                    <div class="upload-icon">📄</div>
                    <div class="upload-text">Click to upload resume file</div>
                    <button class="upload-btn">Choose File</button>
                    <input type="file" id="fileInput" class="file-input" accept=".txt,.pdf,.doc,.docx">
                </div>
                
                <div class="text-input">
                    <textarea id="resumeText" placeholder="Or paste your resume text here..."></textarea>
                </div>
                
                <button class="parse-btn" id="parseBtn">🔍 Parse Resume</button>
            </div>
            
            <div class="results-section">
                <div class="results-title">
                    <span>📊</span>
                    <span>Candidate Profile</span>
                </div>
                
                <div id="candidateScore" style="display: none;">
                    <div class="field-group">
                        <div class="field-label">Candidate Score</div>
                        <div class="field-value" style="display: flex; align-items: center; gap: 15px;">
                            <div class="score-circle" id="scoreCircle">85</div>
                            <div>
                                <div style="font-weight: 600; margin-bottom: 5px;">Strong Match</div>
                                <div style="font-size: 0.9em; color: #666;">Based on skills, experience, and qualifications</div>
                            </div>
                        </div>
                    </div>
                </div>
                
                <div class="loading" id="loading">
                    <div class="spinner"></div>
                    <p>AI is analyzing your resume...</p>
                </div>
                
                <div id="results" style="display: none;">
                    <div class="field-group">
                        <div class="field-label">Full Name</div>
                        <div class="field-value" id="nameField">Not found</div>
                    </div>
                    
                    <div class="field-group">
                        <div class="field-label">Email Address</div>
                        <div class="field-value" id="emailField">Not found</div>
                    </div>
                    
                    <div class="field-group">
                        <div class="field-label">Phone Number</div>
                        <div class="field-value" id="phoneField">Not found</div>
                    </div>
                    
                    <div class="field-group">
                        <div class="field-label">Skills</div>
                        <div class="field-value" id="skillsField">
                            <div class="skills-list" id="skillsList">No skills found</div>
                        </div>
                    </div>
                    
                    <div class="field-group">
                        <div class="field-label">Experience Summary</div>
                        <div class="field-value" id="experienceField">Not found</div>
                    </div>
                    
                    <div class="field-group">
                        <div class="field-label">Education</div>
                        <div class="field-value" id="educationField">Not found</div>
                    </div>
                    
                    <div class="section-divider"></div>
                    
                    <div id="hrRecommendations">
                        <div class="field-group">
                            <div class="field-label">HR Recommendations</div>
                            <div class="recommendation">
                                <h4>🎯 Next Steps</h4>
                                <p id="recommendationText">Complete parsing to see AI-powered recommendations for this candidate.</p>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <script>
        class ResumeParser {
            constructor() {
                this.initializeEventListeners();
            }

            initializeEventListeners() {
                const fileInput = document.getElementById('fileInput');
                const parseBtn = document.getElementById('parseBtn');
                
                fileInput.addEventListener('change', this.handleFileSelect.bind(this));
                parseBtn.addEventListener('click', this.parseResume.bind(this));
            }

            handleFileSelect(event) {
                const file = event.target.files[0];
                if (file) {
                    const reader = new FileReader();
                    reader.onload = (e) => {
                        document.getElementById('resumeText').value = e.target.result;
                    };
                    reader.readAsText(file);
                }
            }

            async parseResume() {
                const resumeText = document.getElementById('resumeText').value.trim();
                
                if (!resumeText) {
                    this.showError('Please upload a file or paste resume text');
                    return;
                }

                this.showLoading();
                
                try {
                    // Simulate AI processing delay
                    await new Promise(resolve => setTimeout(resolve, 2000));
                    
                    const parsedData = this.performAIParsing(resumeText);
                    this.displayResults(parsedData);
                } catch (error) {
                    this.showError('Error parsing resume: ' + error.message);
                } finally {
                    this.hideLoading();
                }
            }

            performAIParsing(text) {
                // Simple AI-like parsing using pattern matching and NLP techniques
                const data = {
                    name: this.extractName(text),
                    email: this.extractEmail(text),
                    phone: this.extractPhone(text),
                    skills: this.extractSkills(text),
                    experience: this.extractExperience(text),
                    education: this.extractEducation(text)
                };

                return data;
            }

            extractName(text) {
                // Look for name patterns at the beginning of resume
                const lines = text.split('\n').map(line => line.trim()).filter(line => line);
                
                // Common name patterns
                const namePatterns = [
                    /^([A-Z][a-z]+ [A-Z][a-z]+(?:\s[A-Z][a-z]+)?)\s*$/,
                    /Name\s*:?\s*([A-Z][a-z]+ [A-Z][a-z]+(?:\s[A-Z][a-z]+)?)/i,
                ];

                // Check first few lines for name
                for (let i = 0; i < Math.min(5, lines.length); i++) {
                    const line = lines[i];
                    for (const pattern of namePatterns) {
                        const match = line.match(pattern);
                        if (match) {
                            return match[1] || match[0];
                        }
                    }
                    
                    // Simple heuristic: if line has 2-3 words, all capitalized, likely a name
                    const words = line.split(/\s+/);
                    if (words.length >= 2 && words.length <= 3 && 
                        words.every(word => /^[A-Z][a-z]+$/.test(word))) {
                        return line;
                    }
                }

                return 'Not found';
            }

            extractEmail(text) {
                const emailPattern = /\b[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Z|a-z]{2,}\b/g;
                const matches = text.match(emailPattern);
                return matches ? matches[0] : 'Not found';
            }

            extractPhone(text) {
                const phonePatterns = [
                    /(\+?\d{1,3}[-.\s]?)?\(?\d{3}\)?[-.\s]?\d{3}[-.\s]?\d{4}/g,
                    /(\+?\d{1,3}[-.\s]?)?\d{10}/g,
                    /Phone\s*:?\s*([\d\s\-\(\)\+\.]+)/i
                ];

                for (const pattern of phonePatterns) {
                    const matches = text.match(pattern);
                    if (matches) {
                        return matches[0].replace(/Phone\s*:?\s*/i, '').trim();
                    }
                }

                return 'Not found';
            }

            extractSkills(text) {
                // Common technical and professional skills
                const skillKeywords = [
                    // Programming languages
                    'JavaScript', 'Python', 'Java', 'C++', 'C#', 'PHP', 'Ruby', 'Go', 'Swift', 'Kotlin',
                    'HTML', 'CSS', 'React', 'Angular', 'Vue', 'Node.js', 'Express',
                    
                    // Databases
                    'MySQL', 'PostgreSQL', 'MongoDB', 'Oracle', 'SQL Server', 'Redis',
                    
                    // Tools and technologies
                    'Git', 'Docker', 'Kubernetes', 'AWS', 'Azure', 'GCP', 'Jenkins',
                    'Selenium', 'Jest', 'JUnit', 'Postman',
                    
                    // Soft skills
                    'Leadership', 'Communication', 'Project Management', 'Problem Solving',
                    'Team Work', 'Critical Thinking', 'Time Management',
                    
                    // Other technical
                    'Machine Learning', 'AI', 'Data Analysis', 'DevOps', 'Agile', 'Scrum'
                ];

                const foundSkills = [];
                const textLower = text.toLowerCase();
                
                // Look for skills section specifically
                const skillsSection = this.extractSection(text, ['skills', 'technical skills', 'core competencies']);
                const searchText = skillsSection || text;

                skillKeywords.forEach(skill => {
                    if (searchText.toLowerCase().includes(skill.toLowerCase())) {
                        foundSkills.push(skill);
                    }
                });

                // Remove duplicates and return
                return [...new Set(foundSkills)];
            }

            extractExperience(text) {
                const experienceSection = this.extractSection(text, [
                    'experience', 'work experience', 'professional experience', 
                    'employment', 'career history'
                ]);

                if (experienceSection) {
                    // Get the first few lines of experience section
                    const lines = experienceSection.split('\n')
                        .map(line => line.trim())
                        .filter(line => line && line.length > 10)
                        .slice(0, 3);
                    
                    return lines.join(' ').substring(0, 200) + '...';
                }

                // Look for job titles and company patterns
                const jobPatterns = [
                    /(?:Software Engineer|Developer|Manager|Analyst|Consultant|Designer|Director)\s+at\s+[\w\s]+/gi,
                    /\d{4}\s*-\s*(?:\d{4}|Present)\s*[:\-]\s*(.+)/gi
                ];

                for (const pattern of jobPatterns) {
                    const matches = text.match(pattern);
                    if (matches) {
                        return matches.slice(0, 2).join('; ');
                    }
                }

                return 'Not found';
            }

            extractEducation(text) {
                const educationSection = this.extractSection(text, [
                    'education', 'academic background', 'qualifications', 'degrees'
                ]);

                if (educationSection) {
                    const lines = educationSection.split('\n')
                        .map(line => line.trim())
                        .filter(line => line && line.length > 5)
                        .slice(0, 2);
                    
                    return lines.join('; ');
                }

                // Look for degree patterns
                const degreePatterns = [
                    /(?:Bachelor|Master|PhD|B\.S\.|M\.S\.|B\.A\.|M\.A\.|B\.Tech|M\.Tech)[\w\s]*(?:in|of)\s+[\w\s]+/gi,
                    /(?:University|College|Institute)[\w\s]+\d{4}/gi
                ];

                for (const pattern of degreePatterns) {
                    const matches = text.match(pattern);
                    if (matches) {
                        return matches.slice(0, 2).join('; ');
                    }
                }

                return 'Not found';
            }

            extractSection(text, sectionNames) {
                const lines = text.split('\n');
                let startIndex = -1;
                let endIndex = lines.length;

                // Find section start
                for (let i = 0; i < lines.length; i++) {
                    const line = lines[i].toLowerCase().trim();
                    for (const sectionName of sectionNames) {
                        if (line.includes(sectionName.toLowerCase()) && line.length < 50) {
                            startIndex = i + 1;
                            break;
                        }
                    }
                    if (startIndex !== -1) break;
                }

                if (startIndex === -1) return null;

                // Find section end (next section or significant gap)
                const commonSections = ['experience', 'education', 'skills', 'projects', 'certifications', 'awards'];
                for (let i = startIndex; i < lines.length; i++) {
                    const line = lines[i].toLowerCase().trim();
                    if (line && commonSections.some(section => 
                        line.includes(section) && line.length < 50 && !sectionNames.includes(section)
                    )) {
                        endIndex = i;
                        break;
                    }
                }

                return lines.slice(startIndex, endIndex).join('\n').trim();
            }

            displayResults(parsedData) {
                document.getElementById('nameField').textContent = parsedData.name;
                document.getElementById('emailField').textContent = parsedData.email;
                document.getElementById('phoneField').textContent = parsedData.phone;
                document.getElementById('experienceField').textContent = parsedData.experience;
                document.getElementById('educationField').textContent = parsedData.education;

                // Calculate and display candidate score
                const score = this.calculateCandidateScore(parsedData);
                document.getElementById('scoreCircle').style.setProperty('--score', score);
                document.getElementById('scoreCircle').textContent = score;
                document.getElementById('candidateScore').style.display = 'block';

                // Display skills with keyword matching
                const skillsList = document.getElementById('skillsList');
                if (parsedData.skills.length > 0) {
                    skillsList.innerHTML = '';
                    parsedData.skills.forEach(skill => {
                        const skillTag = document.createElement('div');
                        skillTag.className = 'skill-tag';
                        skillTag.textContent = skill;
                        skillsList.appendChild(skillTag);
                    });
                } else {
                    skillsList.innerHTML = 'No skills found';
                }

                // Generate HR recommendations
                this.generateHRRecommendations(parsedData);

                document.getElementById('results').style.display = 'block';
            }

            calculateCandidateScore(data) {
                let score = 0;
                
                // Scoring algorithm for HR evaluation
                if (data.name !== 'Not found') score += 10;
                if (data.email !== 'Not found') score += 10;
                if (data.phone !== 'Not found') score += 10;
                if (data.experience !== 'Not found') score += 25;
                if (data.education !== 'Not found') score += 20;
                
                // Skills scoring
                const skillCount = data.skills.length;
                if (skillCount > 0) score += Math.min(skillCount * 2, 25);
                
                return Math.min(score, 100);
            }

            generateHRRecommendations(data) {
                const recommendations = [];
                
                if (data.skills.length >= 5) {
                    recommendations.push("Strong technical profile with diverse skills");
                }
                
                if (data.experience.includes('Manager') || data.experience.includes('Lead')) {
                    recommendations.push("Leadership experience detected");
                }
                
                if (data.email.includes('.edu') || data.education.includes('Master') || data.education.includes('PhD')) {
                    recommendations.push("Strong educational background");
                }
                
                if (recommendations.length === 0) {
                    recommendations.push("Review candidate profile for specific role requirements");
                }
                
                const recommendationText = recommendations.join('. ') + '.';
                document.getElementById('recommendationText').textContent = recommendationText;
            }

            showLoading() {
                document.getElementById('loading').classList.add('show');
                document.getElementById('results').style.display = 'none';
                document.getElementById('parseBtn').disabled = true;
            }

            hideLoading() {
                document.getElementById('loading').classList.remove('show');
                document.getElementById('parseBtn').disabled = false;
            }

            showError(message) {
                const existingError = document.querySelector('.error');
                if (existingError) {
                    existingError.remove();
                }

                const errorDiv = document.createElement('div');
                errorDiv.className = 'error';
                errorDiv.textContent = message;
                document.querySelector('.upload-section').appendChild(errorDiv);

                setTimeout(() => {
                    errorDiv.remove();
                }, 5000);
            }
        }

        // Initialize the resume parser when page loads
        document.addEventListener('DOMContentLoaded', () => {
            new ResumeParser();
        });
    </script>
</body>
</html>
