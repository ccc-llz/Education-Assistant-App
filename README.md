### overview
- The Education Assistant App is an intelligent application designed specifically for students at the **University of Sydney**. 
- Its aim is to help students efficiently schedule their courses to meet both academic and career development needs. 
-  Based intelligent agent, the app will provide personalized course scheduling support and answer a variety of student queries about their courses.  
- Our goal is to release the burden on students while enhancing their academic performance and promoting their overall development during their time at university.  
- At this stage, we have implemented features such as the LLM language assistant, student registration, login and logout, and the ability for students to post messages.

### configuration
- Our project is broadly divided into two parts: the Python files that link to the LLM and the Node.js files that implement the web application.
- **LLM**
- In the configuration section, we implemented prompts to effectively control the chatbot's behavior. By carefully designing these prompts, we can guide the language model's responses and ensure they align with user expectations. This approach enhances the overall interaction quality and provides a more tailored experience for users.
- **python**
  - First, create a virtual environment for the Python files using `python3 -m venv venv`, and activate it with  `source venv/bin/activate`.
  - Install OpenAI in the virtual environment: `pip show openai`.
  - Install Flask in the virtual environment: pip install flask.
  - Other project dependencies or specific versions:
    - Flask (2.0.3)
    - OpenAI (0.10.2)
    - SQLite3 (2.6.0)
- **node.js**
  - Please download and run MongoDB.
  - Then, run the following command in the root directory of the file to install the required dependencies: `npm install`
  - Other project dependencies or detailed versions:
    - @faker-js/faker (8.4.1)
    - axios (1.7.7)
    - bcrypt (5.1.1)
    - express (4.19.2)
    - mongoose (8.5.2)
    - passport (0.7.0)


### deployment instructions
**Local Deployment**
- First, run the Python file: `python your-path/service2.py`.  
- Then, enter the following command in the terminal to start the application: `npm start`.  
- Once the application has successfully started, you will see the terminal output: `Server running on port 4500`.  
- This means you can access it through your browser at `http://localhost:4500`.  
- After a simple registration, you'll be able to access our learning assistant seamlessly!  

**Cloud deployment**
- In addition to local deployment, we also chose a cloud environment to enhance the system's availability and performance, which mainly consists of two steps:
  - **Upload MongoDB: Remote database**: First, we upload the MongoDB database to a remote database service. This means that our database will no longer be stored locally, but will be deployed in the cloud, allowing for easier access and management, while also improving data security and reliability.
  - **Heroku: Cloud service**: Next, we deploy the application to the Heroku cloud service platform. Heroku provides a user-friendly environment that allows us to quickly deploy, manage, and scale our application. Through Heroku, our application can better serve users globally while benefiting from automatic scaling and high availability.
- By implementing these two steps, our application can run stably in the cloud, handle more concurrent requests, and enhance the user experience
- THIS PART ALSO SERVES FOR THE NEXT CHAPTER **"Advanced Technologies Used"**

### Advanced Technologies Used
#### Applicationframeworks 
1. **Node.js**: 
   - Used for building efficient server-side applications. It employs an event-driven, non-blocking I/O model that can handle a large number of concurrent requests, enhancing performance.  
   - **Efficient Operations**  
   - In our project, Node.js enables us to create an efficient chat service that can simultaneously handle requests from multiple users. For example, in the following code snippet, we use Node.js to create an HTTP server:  
        ```javascript
        const http = require('http');
        const server = http.createServer((req, res) => {
            res.statusCode = 200;
            res.setHeader('Content-Type', 'text/plain');
            res.end('Hello World\n');
        });
        server.listen(5000, () => {
            console.log('Server running at http://localhost:5000/');
        });
        ```
   - In this example, Node.js receives requests and returns responses in an event-driven manner. Its non-blocking I/O operations allow the server to continue processing other requests while waiting for an operation (such as a database query) to complete. Thus improving overall throughput.  
   - **Performance Improvement**:  
   - Compared to traditional multi-threaded servers, Node.js can handle more concurrent requests within a single thread. This design makes Node.js particularly suitable for I/O-intensive applications, such as chat services or real-time data processing. In the future, when we implement the notification system, it will enhance the server's performance and responsiveness.  
   
2. **Express.js**：
   - A flexible Node.js web application framework that offers a powerful set of features for building web and mobile applications. This will simplifies routing and middleware management, and enhances maintainability. 
   - We use Express.js to create and manage our web applications. In our project, Express.js is primarily utilized for creating and managing web applications, particularly in the following areas:  
   - **Routing Management**  
   - Express.js allows us to easily define routes to handle different HTTP requests. For example, we can create API endpoints for the chat service as follows:  
        ```javascript
        const authenticate = (req, res, next) => {
            const token = req.headers['authorization'];
            // Validate the token...
            next();
        };
        app.use(authenticate); // Apply authentication middleware
        ```
   - In this example, Express.js allows us to define a POST request endpoint at /chat to receive user chat messages and return a response.  
   - **Middleware Support**  
   - Express.js supports the use of middleware, enabling us to insert custom logic into the request handling process, such as authentication, logging, and more. For example, we added a middleware function to validate user identity:  
        ```javascript
        const authenticate = (req, res, next) => {
            const token = req.headers['authorization'];
            // Validate the token...
            next();
        };
        app.use(authenticate); // Apply authentication middleware
        ```
   - This way, we can perform necessary validations before processing requests, enhancing the security of the application.  
   - **Error Handling**  
   - Express.js provides a convenient error handling mechanism, allowing us to manage errors in the application centrally.  
        ``` javascript
        app.use((err, req, res, next) => {
            console.error(err.stack);
            res.status(500).send('Something broke!');
        });
        ```
   - This code captures all unhandled errors and returns a 500 status code, ensuring that users receive appropriate error messages.  
   - **Scalability and Maintainability**  
   - Due to the modular design of Express.js, we can easily separate different functional modules, enhancing the readability and maintainability of the code. For example, we place route definitions in separate files to further improve the organization of the project:  
        ```JAVASCRIPT
        const chatRoutes = require('./routes/chat');
        app.use('/api', chatRoutes);
        ```
    - With these features, Express.js not only enhances development efficiency in our project but also improves the application's maintainability and scalability, allowing us to quickly respond to user needs and adapt to future feature expansions.

1. **Flask**: A lightweight Python web framework suitable for building simple APIs, it allows for quick request responses and enhances development efficiency.  
   - **Rapidly Set Up Web Services**  
   - Flask enables us to quickly create web services, easily set up routes, and handle requests. The line `app = Flask(__name__)` initializes the Flask application, allowing us to define API endpoints.  
   - **Defining API Endpoints**  
   - With Flask, I can easily create API endpoints to handle user chat requests. For example, in the code, I defined a POST request endpoint at /llm:  
        ```python
        @app.route('/llm', methods=['POST'])
        def chat():
            user_input = request.json.get('message')
            user_id = request.json.get('user_id')

            if not user_input or not user_id:
                return jsonify({"error": "No input or user ID provided"}), 400

            # Process chat and get the response
            response = chat_with_llm(user_id, user_input)

            return jsonify({"reply": response}), 200
        ```
   - This ensures that user inputs can be sent in JSON format, enhancing the application's interactivity and flexibility.  
   - **Integration of Database Interaction**  
   - Flask simplifies the interaction with the SQLite database. I handle database initialization, user chat history retrieval, and message saving by defining the functions `init_db`, `get_user_history`, and `save_message`. This structured approach to database interaction improves the system's maintainability and performance.
        ```python
        def init_db():
            conn = sqlite3.connect('chat_history.db')
            cursor = conn.cursor()
            cursor.execute('''CREATE TABLE IF NOT EXISTS chat_history (
                                id INTEGER PRIMARY KEY AUTOINCREMENT,
                                user_id TEXT NOT NULL,
                                role TEXT NOT NULL,
                                message TEXT NOT NULL
                            )''')
            conn.commit()
            conn.close()
        ```
   - **Encapsulation of Chat Logic**  
   - By encapsulating the chat logic with the large language model (LLM) in the `chat_with_llm` function, the main logic remains clear and easy to manage. This modular design enhances the readability and maintainability of the code.
        ``` python
        def chat_with_llm(user_id, user_input):
            conversation_history = get_user_history(user_id)
            conversation_history.append({"role": "user", "content": user_input})
            chat_completion = openai.chat.completions.create(
                model="gpt-3.5-turbo-16k",
                messages=conversation_history
            )
            response = chat_completion.choices[0].message.content
            save_message(user_id, "user", user_input)
            save_message(user_id, "assistant", response)
            return response
        ```
   - **Error Handling and Feedback**  
   - The code includes validity checks for user input, ensuring that necessary information is provided during API calls, which enhances the robustness of the application.
        ```python
        if not user_input or not user_id:
            return jsonify({"error": "No input or user ID provided"}), 400
        ```
   - **Starting and Running**  
   - Finally, by using `app.run(port=5001)`, the Flask application is launched, allowing the service to run on the specified port, making it easy for users to access the service.  
   - **Conclusion**  
   - The above code demonstrates that Flask provides a structured, scalable, and maintainable way to build chat services. By leveraging these features of Flask, the system can not only respond quickly to user requests but also efficiently handle database interactions, thereby enhancing the overall performance and scalability of the system.
   

#### Database Technologies
1. **MongoDB**: A high-performance, scalable NoSQL database that supports flexible data structures, making it suitable for rapid development and high-concurrency scenarios, thus enhancing system scalability and data processing speed.  
   - MongoDB is a high-performance, scalable NoSQL database that uses a document storage model and supports flexible data structures, making it particularly well-suited for rapid development and high-concurrency scenarios. Its non-relational characteristics allow developers to store complex data structures without a fixed schema, significantly improving system scalability and data processing speed.  
   - In our project, MongoDB is used to store user chat histories and other related data. Its flexible data model enables us to quickly add and modify data structures to meet evolving requirements. For example,
    ```javascript
    const mongoose = require('mongoose');
    const chatSchema = new mongoose.Schema({
        userId: String,
        role: String,
        message: String,
        timestamp: { type: Date, default: Date.now }
    });
    const Chat = mongoose.model('Chat', chatSchema);
    const saveChatMessage = async (userId, role, message) => {
        const chatMessage = new Chat({ userId, role, message });
        await chatMessage.save();
    };
    ```
In this example, we define a schema for chat records and create a Mongoose model to facilitate saving and querying chat messages in the database. This flexibility allows us to quickly adjust the data structure as requirements change.
2. **Mongoose**:
   - Mongoose is an object modeling tool for MongoDB that provides a structured interface for database operations, making interactions with MongoDB more concise and efficient. It offers rich features for defining data models, data validation, querying, and relationships, thus enhancing the efficiency and consistency of data interactions.
   - In our project, Mongoose is primarily used for defining data models, performing CRUD operations, and handling data validation. For example, we can utilize Mongoose's functionality to implement the following:
   - **Defining Data Models**:
   - With Mongoose, we can easily define data models, including field types and validation rules, to ensure data consistency.
        ```javascript
        const userSchema = new mongoose.Schema({
            username: { type: String, required: true },
            email: { type: String, required: true, unique: true },
            password: { type: String, required: true }
        });
        const User = mongoose.model('User', userSchema);
        ```
   - **Data Validation and Operations**:
   - Mongoose provides built-in validation mechanisms that can validate data during insertion or updates, reducing issues caused by data errors.
        ``` javascript
        const createUser = async (username, email, password) => {
            const user = new User({ username, email, password });
            try {
                await user.save();
            } catch (error) {
                console.error('Error creating user:', error);
            }
        };
        ```
   - **Query Operations**:
   - Using Mongoose, querying data becomes simple and efficient, allowing for method chaining to build complex query logic:
        ``` javascript
        const findUserByEmail = async (email) => {
            return await User.findOne({ email });
        };
        ```
   - By integrating MongoDB and Mongoose, our project can efficiently handle a large number of concurrent requests while easily managing and expanding the data structure, thereby enhancing the overall performance and maintainability of the system.

#### New AI Tools or Techniques  
1. **OpenAI API**: Integrating large language models (LLMs) provides intelligent responses and personalized support. We mainly enhance the system's functionality and user experience by interacting with LLMs.
   - **Integrating OpenAI API**: 
   - In the code, we import the OpenAI Python client library using `import openai`. This allows us to utilize various OpenAI models to handle natural language tasks. The API key is set through environment variables to ensure security.
        ```python
        os.environ["OPENAI_API_KEY"] = 'YOUR_OPENAI_API_KEY'
        openai.api_key = os.environ.get("OPENAI_API_KEY")
        ```
   - **Implementation of Chat Functionality**:  
   - In the `chat_with_llm` function, the code facilitates the actual interaction with the OpenAI LLM. This function is responsible for generating responses based on the user's input:
        ```python
        def chat_with_llm(user_id, user_input):
            conversation_history = get_user_history(user_id)
            conversation_history.append({"role": "user", "content": user_input})

            chat_completion = openai.chat.completions.create(
                model="gpt-3.5-turbo-16k",
                messages=conversation_history
            )
            response = chat_completion.choices[0].message.content
        ```
   - **Retrieving User History**: The `get_user_history` function is called first to obtain the user's chat history, providing context information so that the LLM can generate more relevant responses.  
   - **Sending the Request**: Using the `openai.chat.completions.create` method, the user's message and context history are sent to OpenAI, utilizing a specified model (like `gpt-3.5-turbo-16k`) to generate a response.  
   - **Handling the Response**: Once the response from the LLM is received, the code stores it in the database for future reference. This design enhances the system's usability and user experience, as the user's chat history can be continuously tracked and utilized.
        ```python
        save_message(user_id, "user", user_input)
        save_message(user_id, "assistant", response)
        ```
   - **Enhancing User Experience**  
   By integrating the OpenAI API, the system can provide intelligent chat functionality, allowing users to interact with the LLM in natural language. This feature not only increases the interactivity of the application but also enhances the user experience, enabling users to receive personalized and immediate responses.

   - **Summary**  
   The integration of the OpenAI API enables the code to achieve advanced natural language processing capabilities, enhancing the system's intelligence and responsiveness. By leveraging the LLM's contextual understanding, the application can deliver a more human-like interaction experience, which not only helps users obtain information but also increases the overall value of the system.

#### Security Frameworks
1. **Passport Middleware**  
   Passport is a flexible and powerful Node.js middleware designed for user authentication. It supports a variety of authentication strategies, allowing developers to easily integrate user login, registration, and session management features. Due to its flexibility, Passport can be integrated into any Node.js application, providing multiple authentication methods such as local authentication, OAuth, OpenID, and JWT.

   - **Application in Our Project**  
   In our project, the Passport middleware is used to implement user authentication, ensuring that only verified users can access specific functionalities and resources. Here are some specific applications of Passport in the project:
   
   - **Installation and Configuration**  
     We integrate the Passport middleware with our Express application, configuring session management and authentication strategies.
   
   - **User Registration and Login**  
     Next, we set up routes for user registration and login, allowing users to create accounts and log in.

   - **Summary**  
   By integrating the Passport middleware, we can easily implement user authentication, ensuring user security and data privacy. The variety of authentication strategies provided by Passport offers flexibility, enabling us to choose the most suitable verification method based on project requirements. This integration enhances the application's security while maintaining code maintainability.

#### Project Management Techniques

1. **GitHub**  
   GitHub is a version control platform for code management and collaboration that supports teamwork and project management, enhancing development efficiency.

   - **Version Control**  
     With GitHub, team members can track changes in the code and record the history of each commit. This allows us to easily view and revert to previous versions, which helps quickly identify and resolve issues when they arise.

   - **Branch Management**  
     We utilize GitHub's branching feature, enabling each team member to develop on their own branch without affecting the main branch. This approach allows developers to work independently until their features are complete and tested, after which they can merge their code into the main branch through pull requests.

   - **Issue Tracking**  
     We use GitHub's issue tracking feature to manage tasks, bugs, and feature requests. Team members can create and assign issues, clarifying responsibilities and priorities, which helps maintain transparency and manageability in project progress.

   - **Continuous Integration and Deployment (CI/CD)**  
     We leverage GitHub Actions for automated builds and testing. This ensures that after each code submission, the system automatically runs tests, providing rapid feedback on code quality and usability, reducing human errors, and increasing the frequency and reliability of releases.

   - **Summary**  
     Through these features and best practices of GitHub, our team is able to collaborate efficiently, manage projects, improve code quality, and ensure timely delivery. GitHub is not just a tool for code hosting but also an essential platform for our team’s communication and collaboration.
