---
title: Generating a client library from Amazon Ads API v1 specifications
description: Walkthrough of client library generation from OpenAPI specs, including necessary processing scripts and a demonstration client
type: guide
interface: api
tags:
    - Onboarding
    - Authorization
keywords:
    - Ads API v1
    - getting started
    - SDK
    - OpenAPI
    - client library
---

# Generate a client library and SDK from Amazon Ads API specifications 

## Introduction

The Amazon Ads API v1 represents a reimagined approach to the Amazon Ads API, built from the ground up to provide a seamless experience across all Amazon advertising products through a common model. One major benefit of this common model is improved compatibility with code generation tools such as client library generators.

>[Learn more about Amazon Ads API v1.](reference/amazon-ads/overview)

The following walkthrough uses [OpenAPI Generator](https://github.com/OpenAPITools/openapi-generator) to create a client library for campaign management from the Ads API v1 specifications.

>[TIP:Generating an SDK in other languages]Though this walkthrough generates a client library in TypeScript to interface with a TypeScript demonstration client, OpenAPI Generator supports numerous other languages. You can adapt the approach described below to generate your own client libraries and SDKs using OpenAPI Generator or other open-source code generation tool of your choice.

## Prerequisites

### Authentication and authorization

This walkthrough requires that you have already onboarded to the Amazon Ads API as described in our [onboarding overview](guides/onboarding/overview). This enables you to retrieve your **client ID** and **client secret**, which are required for the client application described below.

You should also have retrieved an access token, refresh token, and a profile ID as described in our ["getting started" overview](guides/get-started/overview). The **refresh token** and **profile ID** are required for the client application described below.

### Node

The example code below is configured to use Node 24 in your development environment. Adjustments are required to use earlier versions of Node.

### Java

The `openapi-generator-cli` NPM package is a wrapper around the OpenAPI Generator core, which is a Java project. The generator requires the `java` executable to be available in your `PATH` at a minimum version of JDK 11. For more information on Java as a prerequisite, see the [documentation for the `openapi-generator-cli` package](https://www.npmjs.com/package/@openapitools/openapi-generator-cli).

## Steps

### 1. Set up the workspace

First, create a workspace directory for this demo. 

```bash
# set up the workspace 
cd ~/Desktop # choose a different parent directory if desired
mkdir amazon-ads-workspace
cd amazon-ads-workspace

mkdir packages
cd packages
mkdir amazon-ads-library-demo
mkdir amazon-ads-client-demo
```

The commands above create a workspace with the following structure:

```
amazon-ads-workspace
└── packages
    ├── amazon-ads-library-demo
    └── amazon-ads-client-demo
```

The `amazon-ads-library-demo` directory will include the necessary input and processing scripts to generate a client library from the Amazon Ads API OpenAPI specification. The `amazon-ads-client-demo` directory will contain a demonstration client that uses your generated client library.

#### 1a. Create workspace build scripts

Optionally, you can create a `package.json` in this workspace to add scripts that build each package or the workspace as a whole. This is not required for the steps below.

```json
{
  "name": "amazon-ads-workspace",
  "private": true,
  "workspaces": [
    "packages/*"
  ],
  "scripts": {
    "build": "npm run build --workspaces",
    "build:sdk": "npm run build -w amazon-ads-library-demo",
    "build:client": "npm run build -w amazon-ads-client-demo"
  }
}
```

### 2. Set up the client library package

First, initialize the client library directory as an NPM package, then create two required subdirectories.

```bash
# set up client library package 
cd amazon-ads-library-demo
npm init -y
mkdir input
mkdir scripts
```

#### 2a. Add OAS files

The specifications for Ads API v1 can be downloaded as JSON using the **Download OpenAPI spec** link in the description section of the [specification page](amazon-ads/1-0/openapi). The active selection in the [product selection interface](reference/amazon-ads/getting-started#openapi-specification-interface) determines which specification file is downloaded.

Alternatively, you can download the common specification and the product-specific specifications directly using the following links:

- [Common specification](https://d1y2lf8k3vrkfu.cloudfront.net/openapi/en-us/dest/AmazonAdsAPIALL_prod_3p.json) (`AmazonAdsAPIALL_prod_3p.json`)
- [Sponsored Products specification](https://d1y2lf8k3vrkfu.cloudfront.net/openapi/en-us/dest/AmazonAdsAPISP_prod_3p.json) (`AmazonAdsAPISP_prod_3p.json`)
- [Sponsored Brands specification](https://d1y2lf8k3vrkfu.cloudfront.net/openapi/en-us/dest/AmazonAdsAPISB_prod_3p.json) (`AmazonAdsAPISB_prod_3p.json`)
- [Amazon DSP specification](https://d1y2lf8k3vrkfu.cloudfront.net/openapi/en-us/dest/AmazonAdsAPIDSP_prod_3p.json) (`AmazonAdsAPIDSP_prod_3p.json`)

>[NOTE]The common specification, Sponsored Products specification, and Sponsored Brands specification are **required** for the code examples in the client demo described below. The Amazon DSP specification is not used in the demonstration, but can be used to generate a client library and client using the same methods if your development goals require.

Add these files to the `/input` directory. Your file tree should resemble the following:

```
amazon-ads-workspace
└── packages
    ├── amazon-ads-library-demo
    |   ├── input
    |   |   ├── AmazonAdsAPIALL_prod_3p.json
    |   |   ├── AmazonAdsAPISB_prod_3p.json
    |   |   └── AmazonAdsAPISP_prod_3p.json
    |   └── scripts
    └── amazon-ads-client-demo
```

>[NOTE]Preserve the filenames as they are when downloaded. The processing script described below requires filenames matching the `AmazonAdsAPI.*_prod_3p.json` pattern.

#### 2b. Add the processing script

Given the downloaded OAS files found in `/input`, this script will prune the contracts (i.e., scrub polymorphism, remove auth header parameters) and output a singular client library in the `/generated` directory. 

The unified models will be found directly under `/v1`, while the ad product-specific models will each be found in a subdirectory one level below /v1 (i.e. `/v1/sp`).

<details class="details-bar">
    <summary>
        **Why is additional processing necessary?** Expand this dialog to learn more.
    </summary>

    This script transforms the Amazon Ads API specifications into a well-structured TypeScript client library. It handles multiple API specs, removes problematic polymorphic structures, manages file organization, and ensures proper module exports. The script automates what would otherwise be a complex manual process, creating a consistent and maintainable client library structure that developers can easily use. It's particularly important for maintaining clean type definitions and preventing common issues like naming conflicts or incorrect import paths in the generated code.

    Included functions:

    * `removePolymorphism`: Removes polymorphic structures from OpenAPI schemas to simplify the generated TypeScript code.
    * `renameError`: Renames "Error" objects to "ModelError" to avoid conflicts with JavaScript's built-in Error class.
    * `findInputFiles`: Locates all relevant API specification files that need processing.
    * `getAdProduct`: Extracts the advertising product name from the filename to organize generated code into appropriate directories.
    * `removeRuntimeExports`: Removes duplicate runtime exports from generated files to prevent conflicts.
    * `updateRuntimeImports`: Updates import paths to ensure correct runtime file references across different directory levels.
    * `processSpecFile`: Handles the complete processing pipeline for each spec file, including transformation and code generation.
    * `generateIndexFile`: Creates index files to properly export all generated modules.
    * `setupRootFiles`: Organizes the final directory structure and creates root-level exports.
</details>

```
// amazon-ads-workspace/packages/amazon-ads-library-demo/scripts/processSpecs.cjs
const fs = require('fs');
const path = require('path');
const { execSync } = require('child_process');

// Directory paths
const INPUT_DIR = path.resolve('./input');
const PRUNED_DIR = path.resolve('./pruned');
const GENERATED_DIR = path.resolve('./generated');
const V1_DIR = path.resolve(GENERATED_DIR, 'v1');

// Ensure directories exist
[PRUNED_DIR, GENERATED_DIR, V1_DIR].forEach(dir => {
    if (!fs.existsSync(dir)) {
        fs.mkdirSync(dir, { recursive: true });
    }
});

// Remove polymorphism from schema function
function removePolymorphism(obj) {
    if (typeof obj !== 'object' || obj === null) {
        return obj;
    }

    if (Array.isArray(obj)) {
        return obj.map(item => removePolymorphism(item));
    }

    const newObj = {};
    for (const [key, value] of Object.entries(obj)) {
        newObj[key] = removePolymorphism(value);

        if (key === 'oneOf') {
            const unionedProps = {};

            for (const polyObj of value || []) {
                if (!polyObj.properties) {
                    // Handle reference schemas
                    const schemaRef = polyObj.$ref;
                    if (schemaRef) {
                        const match = schemaRef.match(/#\/components\/schemas\/([a-zA-Z]+)/);
                        if (match) {
                            const schemaName = match[1];
                            const camelCaseName = schemaName.charAt(0).toLowerCase() + schemaName.slice(1);
                            unionedProps[camelCaseName] = polyObj;
                        }
                    }
                } else {
                    Object.assign(unionedProps, polyObj.properties);
                }
            }

            if ('properties' in obj) {
                throw new Error('Polymorphic object already has properties field.');
            }

            newObj.properties = unionedProps;
            delete newObj[key];
        }

        if (key === 'discriminator') {
            delete newObj[key];
        }
    }

    return newObj;
}

// Rename Error object to ModelError
function renameError(obj) {
    if (typeof obj !== 'object' || obj === null) {
        return obj;
    }

    const problemErrorName = 'Error';
    const modelErrorName = 'ModelError';

    obj = replaceErrorSchemaReferences(obj, problemErrorName, modelErrorName);

    const components = obj.components;
    if (components && typeof components.schemas == 'object') {
        const schemas = components.schemas;
        if (schemas[problemErrorName] != null) {
            schemas[modelErrorName] = schemas[problemErrorName];
            delete schemas[problemErrorName];
        }
    }
    return obj;
}

function replaceErrorSchemaReferences(obj, problemErrorName, modelErrorName) {
    if (typeof obj !== 'object' || obj === null) {
        return obj;
    }

    if (Array.isArray(obj)) {
        return obj.map(item => replaceErrorSchemaReferences(item, problemErrorName, modelErrorName));
    }

    const newObj = {};
    for (const [key, value] of Object.entries(obj)) {
        newObj[key] = replaceErrorSchemaReferences(value, problemErrorName, modelErrorName);
        if (key === '$ref' && value === `#/components/schemas/${problemErrorName}`) {
            newObj[key] = `#/components/schemas/${modelErrorName}`;
        }
    }

    return newObj;
}

// Find all input files matching the pattern
function findInputFiles() {
    const files = fs.readdirSync(INPUT_DIR);
    return files.filter(file => file.match(/AmazonAdsAPI.*_prod_3p\.json/));
}

// Extract ad product from filename
function getAdProduct(filename) {
    const match = filename.match(/AmazonAdsAPI(.*)_prod_3p\.json/);
    if (!match || !match[1]) return 'ALL';
    return match[1].toLowerCase();
}

// Process a single spec file
async function removeRuntimeExports(directory) {
    // Remove runtime exports from all .ts files
    const files = fs.readdirSync(directory);
    for (const file of files) {
        if (file.endsWith('.ts')) {
            const filePath = path.join(directory, file);
            let content = fs.readFileSync(filePath, 'utf8');

            // Remove runtime export lines
            content = content.replace(/export \* from ['"]\.\/runtime\.js['"];?\n?/g, '');
            content = content.replace(/import \{[^}]*\} from ['"]\.\/runtime\.js['"];?\n?/g, '');
            content = content.replace(/import \* as runtime from ['"]\.\/runtime\.js['"];?\n?/g, '');

            // Write back the modified content
            fs.writeFileSync(filePath, content);
            console.log(`Removed runtime exports from ${file}`);
        }
    }
}

function updateRuntimeImports(directory, isAllProduct) {
    const processDirectory = (dir) => {
        const files = fs.readdirSync(dir);

        files.forEach(file => {
            const fullPath = path.join(dir, file);
            const stat = fs.statSync(fullPath);

            if (stat.isDirectory()) {
                processDirectory(fullPath);
            } else if (file.endsWith('.ts')) {
                let content = fs.readFileSync(fullPath, 'utf8');

                if (isAllProduct) {
                    // For files in /v1/apis and /v1/models
                    content = content.replace(
                        /from ['"]\.\.\/runtime\.js['"];/g,
                        'from \'../../runtime.js\';'
                    );
                } else {
                    // For files in product-specific directories
                    content = content.replace(
                        /from ['"]\.\.\/runtime\.js['"];/g,
                        'from \'../../../runtime.js\';'
                    );
                }

                fs.writeFileSync(fullPath, content);
            }
        });
    };

    processDirectory(directory);
}

// Updated processSpecFile function
function processSpecFile(filename) {
    const adProduct = getAdProduct(filename);
    console.log(`Processing ${filename} for ad product: ${adProduct}`);

    // Read input file
    const inputPath = path.join(INPUT_DIR, filename);
    const inputSpec = JSON.parse(fs.readFileSync(inputPath, 'utf8'));

    // Process the spec
    let processedSpec = removePolymorphism(inputSpec);
    processedSpec = renameError(processedSpec);

    // Write pruned spec
    const prunedFilename = `pruned_${filename}`;
    const prunedPath = path.join(PRUNED_DIR, prunedFilename);
    fs.writeFileSync(prunedPath, JSON.stringify(processedSpec, null, 2));
    console.log(`Processed spec written to ${prunedPath}`);

    // Determine output directory - ALL goes directly to V1_DIR
    let outputDir = adProduct.toLowerCase() === 'all' ? V1_DIR : path.join(V1_DIR, adProduct);

    if (adProduct.toLowerCase() !== 'all') {
        if (!fs.existsSync(outputDir)) {
            fs.mkdirSync(outputDir, { recursive: true });
        }
    }

    // Run OpenAPI generator
    const generatorCommand = `openapi-generator-cli generate \
-i "${prunedPath}" \
-g typescript-fetch \
-o "${outputDir}" \
--additional-properties="disallowAdditionalPropertiesIfNotPresent=false" \
--additional-properties="enumPropertyNaming=PascalCase" \
--additional-properties="importFileExtension=.js" \
--additional-properties="modelPropertyNaming=camelCase" \
--additional-properties="paramNaming=camelCase" \
--additional-properties="supportsES6=true" \
--additional-properties="useSingleRequestParameter=true" \
--additional-properties="withInterfaces=true" \
--additional-properties="enumUnknownDefaultCase=true" \
--additional-properties="removeEnumValuePrefix=false" \
--additional-properties="sortParamsByRequiredFlag=false" \
--additional-properties="sortModelPropertiesByRequiredFlag=false"`;

    console.log(`Running generator for ${adProduct}`);
    try {
        execSync(generatorCommand, { stdio: 'inherit' });
        console.log(`Successfully generated client library for ${adProduct}`);

        // For non-ALL products, remove runtime.ts and runtime exports
        if (adProduct.toLowerCase() !== 'all') {
            removeRuntimeExports(outputDir);
            // Update imports in product-specific directory
            updateRuntimeImports(outputDir, false);
        } else {
            // Update imports in /v1/apis and /v1/models
            updateRuntimeImports(path.join(V1_DIR, 'apis'), true);
            updateRuntimeImports(path.join(V1_DIR, 'models'), true);
        }

        return adProduct;
    } catch (error) {
        console.error(`Error generating client library for ${adProduct}:`, error);
        return null;
    }
}

// Updated generateIndexFile function
function generateIndexFile(adProducts) {
    const indexPath = path.join(V1_DIR, 'index.ts');

    let indexContent = `// Auto-generated index file\n\n`;

    // Only include runtime export in the main index if we have the ALL product
    if (adProducts.includes('ALL')) {
        indexContent += `export * from './runtime.js';\n`;
    }

    // Add specific exports for each ad product
    adProducts.forEach(product => {
        if (product.toLowerCase() === 'all') {
            // Export base v1 APIs directly from current directory
            indexContent += `export * from './models/index.js';\n`;
            indexContent += `export * from './apis/index.js';\n`;
        } else {
            // Export product-specific APIs from subdirectory
            const exportName = `v1_${product.toLowerCase()}`;
            indexContent += `export * as ${exportName} from './${product.toLowerCase()}/index.js';\n`;
        }
    });

    fs.writeFileSync(indexPath, indexContent);
    console.log(`Generated index file at ${indexPath}`);
}

// Final root file setup for proper imports
function setupRootFiles(adProducts) {
    // Copy runtime.ts to root generated directory
    const v1RuntimePath = path.join(V1_DIR, 'runtime.ts');
    const rootRuntimePath = path.join(GENERATED_DIR, 'runtime.ts');
    if (fs.existsSync(v1RuntimePath)) {
        fs.copyFileSync(v1RuntimePath, rootRuntimePath);
        console.log('Copied runtime.ts to root and removed from v1');
    }

    // Create root index.ts
    const rootIndexPath = path.join(GENERATED_DIR, 'index.ts');
    let rootIndexContent = `// Auto-generated root index file\n\n`;
    rootIndexContent += `export * from './runtime.js';\n`;

    // Sort products to ensure consistent order
    const sortedProducts = [...adProducts].sort((a, b) => {
        // Put 'ALL' related export last
        if (a.toLowerCase() === 'all') return 1;
        if (b.toLowerCase() === 'all') return -1;
        return a.localeCompare(b);
    });

    // Add exports for each product
    sortedProducts.forEach(product => {
        if (product.toLowerCase() === 'all') {
            rootIndexContent += `export * as v1 from './v1/index.js';\n`;
        } else {
            const exportName = `v1_${product.toLowerCase()}`;
            rootIndexContent += `export * as ${exportName} from './v1/${product.toLowerCase()}/index.js';\n`;
        }
    });

    fs.writeFileSync(rootIndexPath, rootIndexContent);
    console.log('Created root index.ts');

    // Update v1/index.ts to only export models and apis
    const v1IndexPath = path.join(V1_DIR, 'index.ts');
    const v1IndexContent = `// Auto-generated v1 index file\n\n` +
        `export * from './models/index.js';\n` +
        `export * from './apis/index.js';\n`;

    fs.writeFileSync(v1IndexPath, v1IndexContent);
    console.log('Updated v1/index.ts');
}

// Main function
function main() {
    const inputFiles = findInputFiles();

    if (inputFiles.length === 0) {
        console.error('No input files found matching the pattern AmazonAdsAPI*_prod_3p.json');
        process.exit(1);
    }

    console.log(`Found ${inputFiles.length} API specification files`);

    // Process each file and collect ad products
    const adProducts = inputFiles
        .map(processSpecFile)
        .filter(product => product !== null);

    // Generate the main index file
    generateIndexFile(adProducts);

    // Set up root files and update v1 structure
    setupRootFiles(adProducts);

    console.log('Processing complete!');
}

// Run the main function
main();
```

#### 2c. Add configuration files

Replace the content of the existing `amazon-ads-library-demo/package.json` with the following:

```json
{
  "name": "amazon-ads-library-demo",
  "version": "1.0.0",
  "engines": { "node": ">=20.0.0" },
  "engineStrict": true,
  "type": "module",
  "main": "./dist/index.js",
  "module": "./dist/index.js",
  "types": "./dist/index.d.ts",
  "scripts": {
    "clean": "rm -rf generated dist",
    "process-specs": "node scripts/processSpecs.cjs",
    "prebuild": "npm run process-specs",
    "build": "tsc",
    "prepare": "npm run clean && npm run build"
  },
  "dependencies": {
  },
  "devDependencies": {
    "typescript": "^5.8.3",
    "@openapitools/openapi-generator-cli": "^2.21.4",
    "@types/node": "^24.0.14"
  },
  "exports": {
    ".": {
      "import": "./dist/index.js",
      "require": "./dist/index.js",
      "types": "./dist/index.d.ts"
    }
  },
  "files": ["dist"]
}
```

Create `amazon-ads-library-demo/tsconfig.json` and add the following configurations:

```json
{
  "compilerOptions": {
    "target": "ES2022",
    "module": "NodeNext",
    "moduleResolution": "NodeNext",
    "lib": ["es2022", "dom", "dom.iterable"],
    "allowJs": true,
    "allowSyntheticDefaultImports": true,
    "declaration": true,
    "emitDeclarationOnly": false,
    "esModuleInterop": true,
    "outDir": "dist",
    "strict": true,
    "noImplicitAny": true,
    "strictNullChecks": false,
    "noImplicitThis": true,
    "alwaysStrict": true,
    "noUnusedLocals": false,
    "noUnusedParameters": false,
    "noImplicitReturns": true,
    "noFallthroughCasesInSwitch": false,
    "inlineSourceMap": true,
    "inlineSources": true,
    "experimentalDecorators": true,
    "strictPropertyInitialization": false,
    "typeRoots": ["./node_modules/@types"],
    "skipLibCheck": true
  },
  "include": ["generated"],
  "exclude": ["node_modules"]
}
```

#### 2d. Generate the client library

Before the _first_ time you generate the client library, install the dependencies specified in `package.json` and create a link to make the built client library available to the client package locally by running the following commands:

```bash
npm install
npm link
```

To generate the client library, either for the first time or to incorporate any updates to the OAS inputs, run:

```bash
npm run build 
```

### 3. Set up the client package

First, initialize the client directory as an NPM package, then create the required subdirectory and a local link to the client library package.

```bash
# set up client package
cd ../amazon-ads-client-demo 
npm init -y
npm link amazon-ads-library-demo # makes client library import available locally
mkdir src
```

#### 3a. Set up auth credentials

As discussed in [Prerequisites](#prerequisites) above, your client application requires credentials to make a successful request to the API. Replace the values for the exported constant variables in the code below with your **client ID**, **client secret**, **refresh token**, and **profile ID**, then add this code to a new file in the `src` directory called `auth_config.ts`:

```
// amazon-ads-workspace/packages/amazon-ads-client-demo/src/auth_config.ts
import axios from 'axios';

export const CLIENT_ID = 'INSERT_CLIENT_ID';
export const CLIENT_SECRET = 'INSERT_CLIENT_SECRET';
export const REFRESH_TOKEN = 'INSERT_REFRESH_TOKEN';
export const PROFILE_ID = 'INSERT_PROFILE_ID';

export async function getAccessToken(): Promise<string> {
    try {
        const response = await axios.post(
            'https://api.amazon.com/auth/o2/token',
            new URLSearchParams({
                grant_type: 'refresh_token',
                client_id: CLIENT_ID,
                refresh_token: REFRESH_TOKEN,
                client_secret: CLIENT_SECRET
            }).toString(),
            {
                headers: {
                    'Content-Type': 'application/x-www-form-urlencoded;charset=UTF-8'
                }
            }
        );

        return response.data.access_token;
    } catch (error) {
        console.error('Error getting access token:', error);
        throw error;
    }
}
```

#### 3b. Create the client application

Next, create a client application in the `src` directory called `client.ts` that uses the generated client library, along with the auth script above, to test several operations:

- use the common `CampaignCreate` model to create a Sponsored Products campaign
- use the `SBCampaignCreate` model to create a Sponsored Brands campaign
- use the `SPCampaignCreate` model to create a Sponsored Products campaign

>[NOTE]All campaigns in the script below are created in `PAUSED` state. To test creation of enabled campaigns, we recommend [creating a test account](guides/account-management/test-accounts/overview).

```
// amazon-ads-workspace/packages/amazon-ads-client-demo/src/client.ts
import {v1, v1_sb, v1_sp, ResponseError, Configuration} from "amazon-ads-library-demo";
import { CLIENT_ID, PROFILE_ID, getAccessToken } from './auth_config.js';

async function createAdClient() {
    const baseConfig = new Configuration({
        basePath: 'https://advertising-api.amazon.com',
        accessToken: getAccessToken,
    });

    return {
        campaigns: new v1.CampaignsApi(baseConfig),
    };
}

async function main() {
    const client = await createAdClient();

    try {
        const unique = "INSERT_UNIQUE_WORD";
        // 1. Create campaign using CampaignCreate model
        const baseCampaign: v1.CampaignCreate = {
            name: `Base Campaign + ${unique}`,
            adProduct: v1.AdProduct.SponsoredProducts,
            state: 'PAUSED',
            // These are the only required fields for CampaignCreate
            // below are populated so no errors
            marketplaceScope: v1.MarketplaceScope.SingleMarketplace,
            marketplaces: [
                v1.Marketplace.Us
            ],
            autoCreationSettings: {
                // Required for SPCampaignCreate
                autoCreateTargets: true
            },
            startDateTime: new Date(),
            budgets: [{
                budgetType: v1.BudgetType.Monetary,
                recurrenceTimePeriod: v1.Recurrence.Daily,
                budgetValue: {
                    monetaryBudgetValue: {
                        monetaryBudget: {
                            value: 1000
                        },
                    },
                }
            }],
        };
        const baseResponse = await client.campaigns.createCampaign({
            amazonAdsClientId: CLIENT_ID,
            amazonAdvertisingAPIScope: PROFILE_ID,
            createCampaignRequest: {
                campaigns: [baseCampaign]
            }
        }).catch(e => {
            if (e.response) {
                console.log("Status: " + e.response.status);
                e.response.json().then((err: ResponseError) =>{
                    console.log("Message: " + err.message);
                });
            }
        });
        console.log('\n\nBase Campaign Response:\n', JSON.stringify(baseResponse, null, 2));

        // 2. Create campaign using SB Campaign model
        const sbCampaign: v1_sb.SBCampaignCreate = {
            name: `Sponsored Brands Campaign + ${unique}`,
            adProduct: v1_sb.SBAdProduct.SponsoredBrands,
            state: 'PAUSED',
            startDateTime: new Date(),
            budgets: [{
                budgetType: v1_sb.SBBudgetType.Monetary,
                recurrenceTimePeriod: v1_sb.SBRecurrence.Daily,
                budgetValue: {
                    monetaryBudgetValue: {
                        monetaryBudget: {
                            value: 1000
                        },
                    },
                }
            }],
            costType: 'CPC',
            marketplaceScope: v1_sb.SBMarketplaceScope.SingleMarketplace,
            marketplaces: [
                v1_sb.SBMarketplace.Us
            ],
            brandId: "A1QZPEZHREUOON",
            optimizations: {
                goalSettings: {
                    kpi: v1_sb.SBKPI.Clicks
                }
            }
            // Required fields for SBCampaignCreate
        };
        const sbResponse = await client.campaigns.createCampaign({
            amazonAdsClientId: CLIENT_ID,
            amazonAdvertisingAPIScope: PROFILE_ID,
            createCampaignRequest: {
                campaigns: [sbCampaign]
            }
        }).catch(e => {
            if (e.response) {
                console.log("Status: " + e.response.status);
                e.response.json().then((err: ResponseError) =>{
                    console.log("Message: " + err.message);
                });
            }
        });
        console.log('\n\nSB Campaign Response:\n', JSON.stringify(sbResponse, null, 2));

        // 3. Create campaign using SP Campaign model
        const spCampaign: v1_sp.SPCampaignCreate = {
            name: `Sponsored Products Campaign + ${unique}`,
            adProduct: v1_sp.SPAdProduct.SponsoredProducts,
            state: 'PAUSED',
            startDateTime: new Date(),
            budgets: [{
                budgetType: v1_sp.SPBudgetType.Monetary,
                recurrenceTimePeriod: v1_sp.SPRecurrence.Daily,
                budgetValue: {
                    monetaryBudgetValue: {
                        monetaryBudget: {
                            value: 1000
                        },
                    },
                }
            }],
            marketplaceScope: v1_sp.SPMarketplaceScope.SingleMarketplace,
            marketplaces: [
                v1_sp.SPMarketplace.Us
            ],
            autoCreationSettings: {
                // Required for SPCampaignCreate
                autoCreateTargets: true
            },
            // Required fields for SPCampaignCreate
        };
        const spResponse = await client.campaigns.createCampaign({
            amazonAdsClientId: CLIENT_ID,
            amazonAdvertisingAPIScope: PROFILE_ID,
            createCampaignRequest: {
                campaigns: [spCampaign]
            }
        }).catch(e => {
            if (e.response) {
                console.log("Status: " + e.response.status);
                e.response.json().then((err: ResponseError) =>{
                    console.log("Message: " + err.message);
                });
            }
        });
        console.log('\n\nSP Campaign Response:\n', JSON.stringify(spResponse, null, 2));

    } catch (error) {
        console.error('Error:', error);
    }
}

main().catch(console.error);
```

#### 3c. Create configuration files

Replace the content of the existing `amazon-ads-client-demo/package.json` with the following:

```json
{
  "name": "amazon-ads-client-demo",
  "version": "1.0.0",
  "engines": { "node": ">=20.0.0" }, 
  "engineStrict": true,
  "description": "",
  "main": "dist/src/client.js",
  "type": "module",
  "scripts": {
    "build": "tsc",
    "start": "node dist/src/client.js",
    "dev": "ts-node --esm src/client.ts"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "dependencies": {
    "@types/node": "^24.0.14",
    "ts-node": "^10.9.2",
    "typescript": "^5.8.3",
    "amazon-ads-library-demo": "^1.0.0",
    "axios": "^1.11.0"
  }
}
```

Create `amazon-ads-client-demo/tsconfig.json` and add the following configurations:

```json
{
  "compilerOptions": {
    "target": "ES2020",
    "module": "ESNext",
    "moduleResolution": "node",
    "esModuleInterop": true,
    "forceConsistentCasingInFileNames": true,
    "strict": true,
    "skipLibCheck": true,
    "outDir": "./dist",
    "rootDir": "."
  },
  "include": ["src/**/*"],
  "exclude": ["node_modules"]
}
```

#### 3d. Build and test the client

Before the _first_ time you build the client, install the dependencies specified in `package.json`:

```bash
npm install
```

To build the client application, either the first time or to incorporate any updates, run:

```bash
npm run build
```

To test the client, run:

```bash
npm run start
```

>[NOTE]Sponsored Products campaign creation requires a unique campaign name. To test the client multiple times without error, use a unique value to replace the string "INSERT\_UNIQUE\_WORD" in `client.ts`. Rebuild the client after each change to this value.

### 4. Response

Each operation in the demo client logs the response to the console. A successful response should produce output similar to the following:

```bash
> amazon-ads-client-demo@1.0.0 build
> tsc

> amazon-ads-client-demo@1.0.0 start
> node dist/src/client.js

Base Campaign Response: 
{
  "success": [
    {
      "index": 0,
      "campaign": {
        "marketplaces": [
          "US"
        ],
        "autoCreationSettings": {
          "autoCreateTargets": true
        },
        "state": "PAUSED",
        "creationDateTime": "2025-07-22T14:34:13.544Z",
        "adProduct": "SPONSORED_PRODUCTS",
        "campaignId": "449940118618577",
        "countries": [
          "US"
        ],
        "tags": [],
        "startDateTime": "2025-07-22T14:34:13.331Z",
        "budgets": [
          {
            "budgetType": "MONETARY",
            "recurrenceTimePeriod": "DAILY",
            "budgetValue": {
              "monetaryBudgetValue": {
                "monetaryBudget": {
                  "currencyCode": "USD",
                  "value": 1000
                }
              }
            }
          }
        ],
        "name": "Base Campaign",
        "marketplaceScope": "SINGLE_MARKETPLACE",
        "lastUpdatedDateTime": "2025-07-22T14:34:13.641Z",
        "optimizations": {
          "bidSettings": {
            "bidStrategy": "SALES_DOWN_ONLY",
            "bidAdjustments": {}
          }
        }
      }
    }
  ],
  "partialSuccess": [],
  "error": []
}

SB Campaign Response: 
{
  "success": [],
  "partialSuccess": [],
  "error": [
    {
      "index": 0,
      "errors": [
        {
          "code": "ACTION_NOT_SUPPORTED",
          "message": "Operation not allowed"
        }
      ]
    }
  ]
}

SP Campaign Response: 
{
  "success": [
    {
      "index": 0,
      "campaign": {
        "marketplaces": [
          "US"
        ],
        "autoCreationSettings": {
          "autoCreateTargets": true
        },
        "state": "PAUSED",
        "creationDateTime": "2025-07-22T14:34:14.475Z",
        "adProduct": "SPONSORED_PRODUCTS",
        "campaignId": "351393774080208",
        "countries": [
          "US"
        ],
        "tags": [],
        "startDateTime": "2025-07-22T14:34:14.308Z",
        "budgets": [
          {
            "budgetType": "MONETARY",
            "recurrenceTimePeriod": "DAILY",
            "budgetValue": {
              "monetaryBudgetValue": {
                "monetaryBudget": {
                  "currencyCode": "USD",
                  "value": 1000
                }
              }
            }
          }
        ],
        "name": "Sponsored Products Campaign",
        "marketplaceScope": "SINGLE_MARKETPLACE",
        "lastUpdatedDateTime": "2025-07-22T14:34:14.605Z",
        "optimizations": {
          "bidSettings": {
            "bidStrategy": "SALES_DOWN_ONLY",
            "bidAdjustments": {}
          }
        }
      }
    }
  ],
  "partialSuccess": [],
  "error": []
}
```

## Next steps

For each OpenAPI specification input, the generated client library exports an object containing both the appropriate models and a set of API classes corresponding to resources in the specification. Each API class includes methods corresponding to available operations in the API.

For instance, in the example client, an instance of `v1.CampaignsApi` is created, then the `v1.CampaignCreate` model is used to supply an argument to the `.createCampaign` method. Similar models and methods are available for each resource included in the OpenAPI specification -- for example, an instance of `v1.AdGroupsApi` will include the `.createAdGroup` method. For each resource, additional methods are available to carry out other operations in the API, such as query, update, or delete requests.

You can adjust the code in `client.ts` to experiment with these other resources and operations, then use this client library in your custom client implementation as required for your development goals. 
