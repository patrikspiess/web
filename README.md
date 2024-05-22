# The Patrik Spiess Web

A web frontend which deploys to surge.

Develop the frontend into the source folder. Use any HTML5, CSS and JS technology. Within this
project I use a simple [bootstrap](https://getbootstrap.com/) page. It is then deployed to surge, a
service for static web publishing for Front-End Developers

## Installing surge

https://surge.sh/help/getting-started-with-surge

## Deploying the Application

    surge source/ --domain https://pspiess.surge.sh


# Auto-Publish with GitHub Actions

To automatically publish this web application a GitHub Workflow (Actions) is used.
See [.github/workflows/deploy.yaml](.github/workflows/deploy.yaml).

## Preparing surge for auto-deployment

To be able to automatically deploy a project to surge you have to generate a token and save your
username (e-mail address) and the token to the respective environment variables.

- Login with your username to surge.sh

    ```
    $ surge login

    Login (or create surge account) by entering email & password.

        email: john.doe@domain
        password:

    Success - Logged in as john.doe@domain.
    ```

- Issue a token for using in the CI pipeline

    ```
    $ surge token

    xc2faf224fa3d96239d3bc2e2623a383e
    ```

    (This token is just en example)

- Set the needed environment variables

    ```
    export SURGE_LOGIN=john.doe@domain
    export SURGE_TOKEN=xc2faf224fa3d96239d3bc2e2623a383e
    ```

    Or set these variables in your CI/CD setting when using it in a pipeline. Make sure to not
    expose the token to any log-files ot output.

- Run the surge deployment task with these variables in your workflow.

    ```yaml
    jobs:
        deploy:
            runs-on: ubuntu-latest
            environment: production
            env:
            SURGE_LOGIN: ${{ vars.SURGE_LOGIN }}
            SURGE_TOKEN: ${{ secrets.SURGE_TOKEN }}
            steps:
            - name: Checkout repository
                uses: actions/checkout@v4
            - name: Install surge
                run: sudo npm install --global surge
            - name: Deploy application
                run: surge source/ --domain https://yourapp.surge.sh
    ```

    Here we used a GitHub environment where we stored the SURGE_LOGIN in a variable and the
    SURGE_TOKEN in a secret.
