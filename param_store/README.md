# Updating, Retrieving, and Deleting Values from AWS Param Store

## Requirements
- Ability to use `aws okta exec`
- Muzzle

## Updating param store variables

1. Update the config file in foundry-harness mapping the path of the parameter to the name of the variable
2. Run `muzzle service gen --app filo` (optional `--service`)
3. Click the PR link that muzzle generates, ensures that it has the config change and the ECS changes
4. Push it up
5. Add your param store variable using muzzle (replace `us-staging` and `us-prod` with what's defined in your local aws config):

```
ax us-staging -- muzzle params push '{ "/app/PARAM_NAME": "param_value" }'
ax us-prod -- muzzle params push '{ "/app/PARAM_NAME": "param_value" }'
```
6. Now you can push the code that references the parameter

## Retrieving parameters

You can get parameters by app with: 
```
ax us-staging -- muzzle params pull /app
ax us-prod -- muzzle params pull /app
```

Or get specific parameters:
```
ax us-staging -- muzzle params pull /app/PARAM_NAME
ax us-prod -- muzzle params pull /app/PARAM_NAME
```

## Deleting parameters

Accidentally put a parameter in the wrong spot? No problem! Delete it with:

```
ax us-staging -- aws ssm delete-parameter --name /app/PARAM_NAME
ax us-prod -- aws ssm delete-parameter --name /app/PARAM_NAME
```
