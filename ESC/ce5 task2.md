### Code Explanation

1. **Route to Add a Department**:

   ```javascript
   router.get('/add/:code', async function(req, res, next) {
       const code = req.params.code;
       console.log(code);
       // Create dept object
       let newDept = new deptmodel.Dept(code);
       // Insert dept object into database
       await deptmodel.insertMany([newDept]);
       // json.stringify the department object as a response to the user
       res.send(`${JSON.stringify(newDept)}`);
   });
   ```

   - **HTTP GET**: This route handles GET requests to `/add/:code`.
   - **Parameter Extraction**: The `code` parameter is extracted from the URL (`req.params.code`).
   - **Logging**: The `code` value is logged to the console.
   - **Creating a Department Object**: A new department object is created using `deptmodel.Dept` with the given code.
   - **Database Insertion**: The new department object is inserted into the database using `deptmodel.insertMany`.
   - **Response**: The newly created department object is converted to a JSON string and sent as the response.

2. **Route to Get All Departments**:

   ```javascript
   router.get('/all/', async function(req, res, next) {
       res.send(`${JSON.stringify(await deptmodel.all())}`);
   });
   ```

   - **HTTP GET**: This route handles GET requests to `/all/`.
   - **Fetching Departments**: It calls `deptmodel.all()` to fetch all departments from the database.
   - **Response**: The fetched departments are converted to a JSON string and sent as the response.

3. **Route to Get All Departments with Staff**:

   ```javascript
   router.get('/all/withstaff/', async function(req, res, next) {
       res.send(`${JSON.stringify(await deptmodel.find('withstaff'))}`);
   });
   ```

   - **HTTP GET**: This route handles GET requests to `/all/withstaff/`.
   - **Fetching Departments with Staff**: It calls `deptmodel.find('withstaff')` to fetch all departments that have associated staff. The exact implementation of this function would depend on the `deptmodel` logic.
   - **Response**: The fetched departments with staff information are converted to a JSON string and sent as the response.

4. **Route to Add a Staff Member**:

   ```javascript
   router.post('/add', async function(req, res, next) {
       const { id, name, code } = req.body;
       
       if (!id || !name || !code) {
           return res.status(400).send('Missing required fields: id, name, code');
       }

       try {
           const newStaff = new staffmodel({ id, name, code });
           await newStaff.save();
           res.status(201).send('Staff added successfully');
       } catch (error) {
           next(error); // Pass errors to the error handler
       }
   });
   ```

   - **HTTP POST**: This route handles POST requests to `/add`.
   - **Request Body Parsing**: The `id`, `name`, and `code` are extracted from the request body (`req.body`).
   - **Validation**: The code checks if all required fields (`id`, `name`, and `code`) are present. If any field is missing, it sends a 400 status response with an error message.
   - **Creating a Staff Object**: A new staff object is created using `staffmodel` with the provided `id`, `name`, and `code`.
   - **Database Insertion**: The new staff object is saved to the database using `await newStaff.save()`.
   - **Response**: If the insertion is successful, it responds with a 201 status and a success message. If there is an error, it passes the error to the next middleware for error handling.

5. **Route to Get All Staff Members**:

   ```javascript
   router.get('/all', async function(req, res, next) {
       try {
           const staffList = await staffmodel.find();
           res.status(200).json(staffList);
       } catch (error) {
           next(error); // Pass errors to the error handler
       }
   });
   ```

   - **HTTP GET**: This route handles GET requests to `/all`.
   - **Fetching Staff Members**: It calls `staffmodel.find()` to fetch all staff members from the database.
   - **Response**: The fetched staff members are sent as a JSON response with a 200 status. If there is an error, it passes the error to the next middleware for error handling.