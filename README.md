# gha2

# How can you access an environment variable corresponding to an input in a Docker container action?
# use the args keyword in the action metadata file to pass the input to the Docker container 

# 1. Your workflow calls the action
    - name: Run my Docker action
      uses: ./
      with:
        username: "Aristide"
      
  GitHub now knows: username = "Aristide"
 
# 2. GitHub loads your action.yml It sees:
     inputs:
       username:
    runs:
      using: docker

  So it prepares to run a Docker container.

# 3. GitHub builds your Dockerfile: It runs:
     docker build -t my-action-image .
     

# 4. GitHub runs the container and injects env vars

        This is the key moment.   
        GitHub runs something equivalent to:
        
     docker run \
      -e INPUT_USERNAME="Aristide" \
      my-action-image 

      
      Two things happen:

        It passes the input as an argument (because you used args:)
        It also injects it as an environment variable (INPUT_USERNAME)
        This is why your entrypoint script can do:

        echo "$INPUT_USERNAME"

