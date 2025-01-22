# **NFL Giants Schedule API**

This repository contains a containerized REST API built with Python and Flask that retrieves the New York Giants' NFL schedule from an external API. The application is designed for deployment on AWS services, including **Elastic Container Service (ECS)**, **API Gateway**, and **Elastic Load Balancer (ELB)**.

---

## **Features**
- Fetches the New York Giants' NFL game schedule for the current or upcoming seasons.
- Exposes a single API endpoint for schedule retrieval.
- Built with Flask and containerized using Docker.
- Supports scalable deployment via AWS ECS with API Gateway integration.

---

## **Prerequisites**
1. **NFL Data API Key**: Obtain an API key from a provider like [Sportsdata.io](https://sportsdata.io) or [Sportradar](https://developer.sportradar.com/).
2. **AWS CLI**: Installed and configured on your local machine.
3. **Docker**: Installed for containerization.

---

## **Getting Started**

### **1. Clone the Repository**
```bash
git clone https://github.com/ehrconsultantforhire/NFL-Giants-Schedule-API.git
cd NFL-Giants-Schedule-API
```

### **2. Set Up Environment Variables**
Create a `.env` file in the root directory and add your API key:
```plaintext
NFL_API_KEY=your_external_api_key
```

Update the `app.py` to load the key from the environment:
```python
import os
API_KEY = os.getenv('NFL_API_KEY')
```

### **3. Install Dependencies**
If running locally, install the required Python packages:
```bash
pip install -r requirements.txt
```

---

## **Running the Application**

### **1. Run Locally**
1. Start the application:
   ```bash
   python app.py
   ```
2. Access the API at:
   ```
   http://localhost:5000/giants/schedule
   ```

### **2. Run with Docker**
1. Build the Docker image:
   ```bash
   docker build -t giants-schedule-api .
   ```
2. Run the container:
   ```bash
   docker run --env-file .env -p 5000:5000 giants-schedule-api
   ```
3. Access the API at:
   ```
   http://localhost:5000/giants/schedule
   ```

---

## **Deployment on AWS**

### **1. Push Docker Image to Amazon ECR**
1. Authenticate Docker with ECR:
   ```bash
   aws ecr get-login-password --region <region> | docker login --username AWS --password-stdin <account_id>.dkr.ecr.<region>.amazonaws.com
   ```
2. Tag the image:
   ```bash
   docker tag giants-schedule-api:latest <account_id>.dkr.ecr.<region>.amazonaws.com/giants-schedule-api:latest
   ```
3. Push the image:
   ```bash
   docker push <account_id>.dkr.ecr.<region>.amazonaws.com/giants-schedule-api:latest
   ```

### **2. Deploy the Application**
1. Create an ECS cluster.
2. Define a task using the Docker image from ECR.
3. Create an ECS service and attach it to an Application Load Balancer.
4. Configure API Gateway to expose the `/giants/schedule` endpoint.

---

## **API Documentation**

### **Endpoint**
**GET** `/giants/schedule`

### **Request Parameters**
None.

### **Sample Response**
```json
[
  {
    "GameKey": "2024REG1",
    "Date": "2024-09-08T13:00:00Z",
    "HomeTeam": "New York Giants",
    "AwayTeam": "Dallas Cowboys",
    "Stadium": "MetLife Stadium",
    "Channel": "FOX",
    "Week": 1
  },
  {
    "GameKey": "2024REG2",
    "Date": "2024-09-15T13:00:00Z",
    "HomeTeam": "Arizona Cardinals",
    "AwayTeam": "New York Giants",
    "Stadium": "State Farm Stadium",
    "Channel": "CBS",
    "Week": 2
  }
]
```

---

## **Project Structure**
```
NFL-Giants-Schedule-API/
├── app.py            # Main Flask application
├── Dockerfile        # Docker configuration
├── requirements.txt  # Python dependencies
├── README.md         # Documentation
├── .env              # Environment variables (not committed)
└── .gitignore        # Ignore sensitive files and build artifacts
```

---

## **Testing**
Add test cases to verify API functionality:
1. Install `pytest`:
   ```bash
   pip install pytest
   ```
2. Create test cases in a `tests/` directory.
3. Run tests:
   ```bash
   pytest
   ```

---

## **Contributing**
Contributions are welcome! Please fork this repository and submit a pull request.

---

## **License**
This project is licensed under the MIT License. See the `LICENSE` file for details.

---

## **Acknowledgments**
- Flask: Lightweight web framework.
- Sportsdata.io: NFL data provider.
- AWS: Hosting and deployment platform.
