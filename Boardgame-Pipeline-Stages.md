**BoardGame Jenkins Pipeline Stage View.**

To summarise, our pipeline stages are as following:

1 - Declarative: Tool Install
2 - Git Checkout
3 - Compile
4 - Test
5 - File System Scan
6 - SonarQube Analysis
7 - Quality Gate
8 - Build
9 - Publish to Nexus
10 - Build & Tag Docker Image
11 - Docker Image Scan
12 - Push Docker Image
13 - Deploy To Kubernetes
14 - Verify the Deployment
15 - Declarative: Post Actions
