# Insured Assurance - Complete CI/CD Pipeline Project

## Author: Ahad Alam

[![Build Status](https://github.com/YOUR_USERNAME/insured-assurance/workflows/CI%2FCD%20Pipeline%20with%20Jenkins%20Integration/badge.svg)](https://github.com/YOUR_USERNAME/insured-assurance/actions)
[![Java Version](https://img.shields.io/badge/Java-17-orange.svg)](https://openjdk.java.net/projects/jdk/17/)
[![Maven](https://img.shields.io/badge/Maven-3.x-blue.svg)](https://maven.apache.org/)
[![Tomcat](https://img.shields.io/badge/Tomcat-9.0.108-yellow.svg)](https://tomcat.apache.org/)

## 📋 Table of Contents
- [Overview](#overview)
- [Architecture](#architecture)
- [Prerequisites](#prerequisites)
- [Quick Start](#quick-start)
- [Pipeline Workflow](#pipeline-workflow)
- [Project Structure](#project-structure)
- [Configuration](#configuration)
- [Deployment](#deployment)
- [Monitoring](#monitoring)
- [Troubleshooting](#troubleshooting)
- [Contributing](#contributing)

## 🌟 Overview

**Insured Assurance** is a demonstration project showcasing a complete DevOps CI/CD pipeline implementation using industry-standard tools and best practices. The project integrates GitHub Actions for Continuous Integration (CI) with Jenkins for Continuous Deployment (CD), automatically deploying a Java web application to Apache Tomcat running on AWS EC2.

### Key Features
- ✅ **Automated CI/CD Pipeline**: Zero-touch deployment from code commit to production
- 🔒 **Security First**: Token-based authentication and secure credential management
- 📊 **Performance Optimized**: Maven dependency caching and efficient resource utilization
- 🚀 **Cloud Native**: Deployed on AWS EC2 with proper security configurations
- 📱 **Modern Tools**: GitHub Actions, Jenkins, Maven, Tomcat integration

## 🏗️ Architecture

```
┌─────────────────┐    ┌──────────────────┐    ┌─────────────────┐
│                 │    │                  │    │                 │
│   GitHub Repo   │───▶│  GitHub Actions  │───▶│    Jenkins      │
│                 │    │       (CI)       │    │      (CD)       │
└─────────────────┘    └──────────────────┘    └─────────────────┘
                                                        │
                                                        ▼
┌─────────────────┐    ┌──────────────────┐    ┌─────────────────┐
│                 │    │                  │    │                 │
│   AWS EC2       │◀───│     Maven        │◀───│   Apache        │
│   Instance      │    │     Build        │    │   Tomcat        │
└─────────────────┘    └──────────────────┘    └─────────────────┘
```

### Technology Stack
- **Cloud Infrastructure**: AWS EC2 (Amazon Linux 2023)
- **CI Platform**: GitHub Actions
- **CD Platform**: Jenkins LTS
- **Build Tool**: Apache Maven 3.x
- **Application Server**: Apache Tomcat 9.0.108
- **Runtime**: Java 17 (Amazon Corretto)
- **Version Control**: Git/GitHub

## 📋 Prerequisites

Before setting up this project, ensure you have:

- **AWS Account** with EC2 access
- **GitHub Account** with repository creation permissions
- **Basic Linux Knowledge** for server administration
- **SSH Client** for EC2 instance access

### System Requirements (Minimum)
- **Instance Type**: t2.micro (1 vCPU, 1 GB RAM)
- **Storage**: 8 GB GP3 EBS volume
- **Network**: Public subnet with internet access
- **Ports**: 22 (SSH), 8080 (Jenkins), 8081 (Tomcat)

## 🚀 Quick Start

### 1. Clone the Repository
```bash
git clone https://github.com/ahad621-simplilearn/insured-assurance.git
cd insured-assurance
```

### 2. Infrastructure Setup
Launch an AWS EC2 instance with the following specifications:
- **AMI**: Amazon Linux 2023
- **Instance Type**: t2.micro
- **Security Groups**: SSH (22), Custom TCP (8080, 8081)

### 3. Environment Setup
Connect to your EC2 instance and run:
```bash
# Update system
sudo dnf update -y

# Install required tools
sudo dnf install -y java-17-amazon-corretto maven git wget unzip

# Verify installations
java -version && mvn -version && git --version
```

### 4. Jenkins Installation
```bash
# Add Jenkins repository
sudo wget -O /etc/yum.repos.d/jenkins.repo \
    https://pkg.jenkins.io/redhat-stable/jenkins.repo
sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io-2023.key

# Install and start Jenkins
sudo dnf install -y jenkins
sudo systemctl enable jenkins
sudo systemctl start jenkins
```

### 5. Tomcat Setup
```bash
# Download and install Tomcat
cd /opt
sudo wget https://dlcdn.apache.org/tomcat/tomcat-9/v9.0.108/bin/apache-tomcat-9.0.108.tar.gz
sudo tar -xzf apache-tomcat-9.0.108.tar.gz
sudo mv apache-tomcat-9.0.108 tomcat9
sudo chmod +x tomcat9/bin/*.sh

# Start Tomcat
sudo /opt/tomcat9/bin/startup.sh
```

## 🔄 Pipeline Workflow

### GitHub Actions (CI)
1. **Code Checkout**: Retrieves latest source code
2. **Environment Setup**: Configures JDK 17 environment
3. **Dependency Management**: Caches Maven dependencies
4. **Build Process**: Validates → Compiles → Tests → Packages
5. **Jenkins Trigger**: Initiates deployment job via API

### Jenkins (CD)
1. **Source Retrieval**: Pulls code from GitHub repository
2. **Build Execution**: Runs Maven package goal
3. **Artifact Generation**: Creates deployable WAR file
4. **Automated Deployment**: Deploys to Tomcat container
5. **Verification**: Confirms successful deployment

## 📁 Project Structure

```
insured-assurance/
├── .github/
│   └── workflows/
│       └── maven.yml              # GitHub Actions workflow
├── src/
│   ├── main/
│   │   ├── java/                  # Java source code
│   │   └── webapp/
│   │       ├── index.jsp          # Main application page
│   │       └── WEB-INF/
│   │           └── web.xml        # Web application descriptor
│   └── test/
│       └── java/                  # Unit tests
├── target/                        # Build output (generated)
├── pom.xml                        # Maven project configuration
└── README.md                      # This file
```

## ⚙️ Configuration

### GitHub Secrets
Configure the following secrets in your GitHub repository:
- `JENKINS_URL`: Your Jenkins server URL
- `JENKINS_USER`: Jenkins username
- `JENKINS_TOKEN`: Jenkins API token
- `JENKINS_JOB_TOKEN`: Job-specific trigger token

### Jenkins Configuration
1. **Global Tools**: Configure Maven 3.x
2. **Plugins**: Install "Deploy to container" plugin
3. **Credentials**: Add GitHub and Tomcat credentials
4. **Job Setup**: Create freestyle project with Git integration

### Tomcat Configuration
1. **Port Configuration**: Modified to use port 8081
2. **Manager App**: Configured with deployment credentials
3. **Remote Access**: Enabled for automated deployments

## 🚀 Deployment

### Manual Deployment
```bash
# Build the project
mvn clean package

# Deploy to Tomcat
cp target/insured-assurance.war /opt/tomcat9/webapps/
```

### Automated Deployment
Simply push code to the `main` branch:
```bash
git add .
git commit -m "Your commit message"
git push origin main
```

The pipeline will automatically:
1. Run GitHub Actions CI
2. Trigger Jenkins CD
3. Deploy to Tomcat
4. Verify deployment

## 📊 Monitoring

### Application Access
- **Main Application**: `http://YOUR_EC2_IP:8081/insured-assurance`
- **Tomcat Manager**: `http://YOUR_EC2_IP:8081/manager`
- **Jenkins Dashboard**: `http://YOUR_EC2_IP:8080`

### Build Status
- Monitor GitHub Actions in the repository's Actions tab
- Check Jenkins build history for deployment status
- Review Tomcat logs for runtime issues

## 🔧 Troubleshooting

### Common Issues

#### Build Failures
```bash
# Check Java version
java -version

# Verify Maven installation
mvn -version

# Clean and rebuild
mvn clean compile
```

#### Deployment Issues
```bash
# Check Tomcat status
sudo /opt/tomcat9/bin/catalina.sh version

# Review Tomcat logs
tail -f /opt/tomcat9/logs/catalina.out

# Restart Tomcat
sudo /opt/tomcat9/bin/shutdown.sh
sudo /opt/tomcat9/bin/startup.sh
```

#### Jenkins Connectivity
```bash
# Check Jenkins status
sudo systemctl status jenkins

# Restart Jenkins
sudo systemctl restart jenkins

# Check Jenkins logs
sudo journalctl -u jenkins -f
```

### Performance Optimization
- **Maven Caching**: Implemented in GitHub Actions workflow
- **Resource Management**: Optimized for t2.micro instance
- **Build Efficiency**: Parallel execution where possible

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

### Development Guidelines
- Follow Maven project structure conventions
- Write unit tests for new functionality
- Update documentation for configuration changes
- Test pipeline integration before submitting PR

## 📈 Metrics and Performance

### Deployment Metrics
- **Build Time**: ~2-3 minutes (with caching)
- **Deployment Time**: ~1-2 minutes
- **Total Pipeline Duration**: ~3-5 minutes
- **Success Rate**: 95%+ after optimization

### Resource Utilization
- **Memory Usage**: ~800MB peak during build
- **CPU Usage**: ~70% during Maven compilation
- **Storage**: ~2GB for complete environment

## 🔮 Future Enhancements

### Planned Features
- [ ] Docker containerization
- [ ] Kubernetes deployment
- [ ] Multi-environment support (dev/staging/prod)
- [ ] Database integration
- [ ] Monitoring with Prometheus/Grafana
- [ ] Security scanning with OWASP
- [ ] Performance testing integration

### Scalability Roadmap
- Load balancing configuration
- Auto-scaling groups
- Blue-green deployments
- Infrastructure as Code (Terraform)

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 📞 Support

For support and questions:
- Create an issue in this repository
- Check the troubleshooting section above
- Review Jenkins and GitHub Actions documentation

---

**Built with ❤️ using modern DevOps practices and tools**
