Install the redux tool kit and redux from react using the following command:-
*npm install @reduxjs/toolkit react-redux*
### to create store: ###
    1.import configureStore from redux toolkit
    2.import reducer from slice 
    3. create store as 
        export const store=configureStore({
            reducer:{
                nameForTheReducer: put slice here,
            }
        })
    4.make sure to provide the store in index.js

 ### To create a slice: ### 
    1. import createslice from redux toolkit
    2. declare initial state
    3. create slice with 
        const (slicename)=createSlice({
            name:"some name",
            initialstate,
            reducers{}
        })
    4. export as ---> export default slicename.reducer 
    5. import the slices to the store.js and put it in configurestore
 ### To deal with async function we can use Thunk for example fetch data from an API  ### 
  ##### (use axios to do the fetch using get and swnd data using post)use npm i axios to installl the dependency ##### 
    
    1. Import createThunk from redux-toolkit
    2. create a function fetchPosts using createAsyncThunk
    3. createAsyncThunk accepts 2 arguments a string that depicts the ction it does and the second is an async callback which return a promise
    4. export const fetchPosts = createAsyncThunk("posts/fetchPosts", async () => {
            try {
                const response = await axios.get(POSTS_URL); //include await in an async function
                return [...response.data];
            } catch (err) {
                return err.message;
            }
        });   
    5. note that the fecting function happens outside the createSlice function and hence the further actions that
       happens outside the slice needs to be handled by a extra reducer
    6. The extra reducer accepts a parameter of the name builder which is an object that handles different cases of a promise  
    7. Since we fetch data from an api, the initial state looks like:
        const initialState = {
            posts: [],// the empty initial state
            status: "idle", //status of the fetching
            error: null, //initial state of error
        };
    8.  An extra reducer example looks like:
        extraReducers(builder) {
            builder
            .addCase(fetchPosts.pending, (state, action) => { // case for when promise pending
                state.status = "loading";
            })
            .addCase(fetchPosts.fulfilled, (state, action) => { //case fow when promise fullfilled
                state.status = "succeeded";
                let min = 1;
                const loadedPosts = action.payload.map(post => {
                post.date = sub(new Date(), { minutes: min++ }).toISOString(); //adding feilds which are not fetched from the dummy api
                post.reactions = {
                    thumbsUp: 0,
                    wow: 0,
                    heart: 0,
                    rocket: 0,
                    coffee: 0,
                }
                return post;
                });
                state.posts = state.posts.concat(loadedPosts);
            }) 
            .addCase(fetchPosts.rejected,(state,action)=>{//case when promise rejects
                state.status='failed'
                state.error=action.error.message  
            })
        },
    9. An extra reducer comes after the reducer in a slice file    
