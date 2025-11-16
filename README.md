# 🚑 Smart Ambulance Triage System

<div align="center">

![Python](https://img.shields.io/badge/Python-3.8+-blue.svg)
![Streamlit](https://img.shields.io/badge/Streamlit-1.28+-red.svg)
![scikit-learn](https://img.shields.io/badge/scikit--learn-1.0+-orange.svg)
![License](https://img.shields.io/badge/License-MIT-green.svg)

**AI-powered emergency medical triage with intelligent ambulance dispatch**

</div>

---

![UI-3](https://github.com/user-attachments/assets/a2ccac81-8c86-41c5-92e8-92aecccfaee9)
![UI-3](https://github.com/user-attachments/assets/62db4690-0bc8-4670-8fd9-61006021ea47)
![UI-3](https://github.com/user-attachments/assets/cfb48d70-abeb-45a8-b85d-ffad49770015)
![UI-3](https://github.com/user-attachments/assets/c555ac7d-ba12-4656-b40e-2e916641866e)

---

## 🌟 Overview

An intelligent emergency response platform that uses **hybrid AI classification** (rule-based + Decision Tree ML) to prioritize ambulance dispatch. Features real-time triage, dispatcher dashboard, driver interface, and Gemini AI chatbot integration.

### Key Features

- ⚡ **Instant Critical Detection** - Life-threatening conditions identified in <1ms
- 🤖 **AI-Powered Diagnosis** - Decision Tree ML on 400-sample dataset
- 💬 **Gemini Chatbot** - Medical guidance via Google Gemini API
- 📊 **Real-time Queue** - Dynamic priority-based dispatch system
- 🎯 **95%+ Accuracy** - Validated on synthetic emergency data

---

## 🛠️ Technology Stack

| Component | Technology |
|-----------|------------|
| **Backend** | Python 3.8+ |
| **Frontend** | Streamlit |
| **ML Model** | Decision Tree (scikit-learn) |
| **AI Chatbot** | Google Gemini API |
| **Storage** | JSON (queue, stats, fleet) |

---

## 📦 Installation

### Prerequisites
- Python 3.8+
- Google Gemini API key

### Setup

```bash
# 1. Clone repository
git clone https://github.com/yourusername/smart-ambulance-triage.git
cd smart-ambulance-triage

# 2. Install dependencies
pip install -r requirements.txt

# 3. Configure API key (create .env file)
echo "GEMINI_API_KEY=your-api-key-here" > .env

# 4. Train ML model
python Triage_Model.ipynb  # or use pre-trained emergency_triage_model.pkl

# 5. Run application
streamlit run index.py
```

**requirements.txt:**
```txt
streamlit>=1.28.0
scikit-learn>=1.0.0
numpy>=1.21.0
pandas>=1.3.0
google-generativeai>=0.3.0
python-dotenv>=1.0.0
```

---

## 🚀 Usage

### Application Access
Open `http://localhost:8501` and choose your role:

1. **👤 Patient** - Request emergency ambulance
2. **👨‍⚕️ Dispatcher** - Manage emergency queue
3. **🚗 Technician/Driver** - Handle assigned cases

### Workflow

```
Patient Request → Critical Screening (3 questions) → Full Assessment (10 features)
                                    ↓
                    Hybrid Classification (Rule + ML + Fallback)
                                    ↓
              Priority Assignment (HIGH/MEDIUM/LOW) + Severity Score
                                    ↓
                        Dispatcher Queue → Ambulance Dispatch
```

---

## 📊 Dataset & Model

### Dataset Details
- **Size**: 400 manually created samples
- **Features**: 10 binary symptom indicators
- **Classes**: 8 emergency conditions
- **Source**: `balanced_emergency_triage_dataset.csv`

### 10 Features
```
1. chest_pain              6. weakness (one-sided)
2. shortness_of_breath     7. seizure
3. unconsciousness         8. trauma
4. bleeding                9. dizziness
5. confusion              10. cyanosis (blue skin)
```

### 8 Diagnosis Classes
| Priority | Condition | Score |
|----------|-----------|-------|
| HIGH | Cardiac Arrest | 150 |
| HIGH | Heart Attack | 135 |
| HIGH | Severe Respiratory Distress | 130 |
| HIGH | Major Trauma/Bleeding | 125 |
| HIGH | Stroke | 120 |
| MEDIUM | Shock/Collapse | 90 |
| MEDIUM | Seizure/Post-Seizure | 85 |
| LOW | Fainting/Syncope | 50 |

### Model Performance
- **Algorithm**: Decision Tree Classifier
- **Training Accuracy**: 96%
- **Test Accuracy**: 95.2%
- **Inference Time**: <10ms

---

## 🔄 Hybrid Classification System

### Three-Phase Approach

**Phase 1: Critical Rules** (Instant - <1ms)
- 7 life-threatening patterns → Immediate HIGH priority
- Example: `unconscious + cyanosis → Cardiac Arrest (150)`

**Phase 2: ML Model** (10-50ms)
- Decision Tree classification for complex cases
- Uses all 10 features for pattern recognition

**Phase 3: Rule-Based Fallback** (<5ms)
- Weighted scoring if ML unavailable
- Ensures system always works

```python
# Example Classification
if unconsciousness == 1 and cyanosis == 1:
    return 'Cardiac Arrest', 'HIGH', 150  # Phase 1
else:
    diagnosis = model.predict(features)    # Phase 2
    # Or fallback scoring if model fails    # Phase 3
```

---

## 💬 Chatbot Integration

### Google Gemini API
- **Model**: Gemini 1.5 Flash
- **Features**: Medical Q&A, symptom guidance, emergency protocols
- **Access**: Click chat icon in any portal

### Example Queries
- "What should I do for chest pain?"
- "How to recognize a stroke?"
- "First aid for bleeding"

---

## 📁 Project Structure

```
TRIAGE/
│
├── .ipynb_checkpoints/
│   └── Triage_Model-checkpoint.ipynb
│
├── pages/
│   ├── chatbot.py              # Gemini AI chatbot
│   ├── patient.py              # Patient emergency portal
│   ├── routing.py              # Dispatcher dashboard
│   └── technician.py           # Ambulance driver interface
│
├── .env                        # API keys (git-ignored)
├── .gitignore
├── balanced_emergency_triage_dataset.csv  # Training data (400 samples)
├── emergency_queue.json        # Real-time patient queue
├── emergency_triage_model.pkl  # Trained Decision Tree model
├── fix_model.py                # Model compatibility fixer
├── fleet_status.json           # Ambulance availability
├── index.py                    # Main app & login
├── requirements.txt            # Python dependencies
├── system_stats.json           # System analytics
├── Triage_Dataset_Balanced.ipynb  # Data exploration
└── Triage_Model.ipynb          # Model training notebook
```

---

## 🎯 Key Components

### Patient Portal (`pages/patient.py`)
- 3-question critical triage
- 10-feature symptom assessment
- AI diagnosis + priority assignment
- Estimated arrival time

### Dispatcher Dashboard (`pages/routing.py`)
- Real-time emergency queue
- Priority-sorted patient list
- One-click ambulance dispatch
- System statistics

### Technician Interface (`pages/technician.py`)
- Assigned patient details
- Status updates (En Route → Arrived → Completed)
- GPS navigation integration
- Patient contact information

### AI Chatbot (`pages/chatbot.py`)
- Powered by Google Gemini 1.5 Flash
- Medical knowledge base
- Natural language Q&A
- Emergency guidance

---

## 🔮 Future Enhancements

- [ ] GPS tracking for real-time ambulance location
- [ ] Multi-language support
- [ ] Voice input for symptoms
- [ ] Hospital bed availability integration
- [ ] Mobile app (React Native)
- [ ] Deep learning models (LSTM/CNN)

---

## 🤝 Contributing

1. Fork the repository
2. Create feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit changes (`git commit -m 'Add AmazingFeature'`)
4. Push to branch (`git push origin feature/AmazingFeature`)
5. Open Pull Request

---

## 📄 License

MIT License - see [LICENSE](LICENSE) file for details.

---

## 📞 Contact

- **Email**: aaryantamhane29@gmail.com
- **GitHub Issues**: [Report Bug](https://github.com/SweetSalt29/smart-ambulance-triage/issues)

---

<div align="center">

**⭐ Star this repo if you find it helpful! ⭐**

Made with ❤️ for emergency medical services

</div>
