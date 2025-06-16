 

## Overview

The Dialoggroup Hunters Insurance Calculator is a Django application that calculates insurance premiums for hunters, with support for various coverage areas (World, Europe, Belgium, France) and different liability amounts.

  

## Features

- Multiple liability coverage options (3M and 6M)

- Territory-based coverage (World, Europe, Belgium, France)

- Duration options (1 year and 3 years)

- Additional coverage options for specific countries

- Active membership verification

- Admin interface for premium management

- Automatic NOJG membership synchronization

  

## Authentication Flow

  

### 1. Authentication System

- Uses custom DialoggroupPermission class

- Token-based authentication

- Scope-based access control

- API key protection for webhooks

  

## API Endpoints

  

### GET `/dialoggroup/jagers`

Retrieve all available liability coverages.

  

**Response:**

```json

[

    {

        "liability_code": "wld_6m_3j",

        "insured_amount": 6000000,

        "area": "Wereld",

        "premium_excl_AB": 247.32,

        "duration_years": 3,

        "base_coverage": false

    }

]

```

  

### POST `/dialoggroup/jagers`

Calculate insurance rates based on selected coverages.

  

**Request:**

```json

{

    "liability_codes": ["wld_6m_3j", "fa_ad_1j"]

}

```

Response:

```json

{

    "coverages": [

        {

            "liability_code": "eu_6m_3j",

            "insured_amount": 6000000,

            "area": "Europa",

            "premium_excl_AB": 153.1,

            "cost_excl_ab": 155.79,

            "duration_years": 3,

            "assurance_tax": 32.72,

            "provision_aon": 15.31,

            "total_coverage_cost": 188.51,

            "additional_coverage": false,

            "policy_costs_hienfeld": 2.69,

            "contribution_guarantee_fund": 0.0

        },

        {

            "liability_code": "be_ad_1j",

            "insured_amount": null,

            "area": "België",

            "premium_excl_AB": 28.6,

            "cost_excl_ab": 28.6,

            "duration_years": 1,

            "assurance_tax": 6.01,

            "provision_aon": 2.86,

            "total_coverage_cost": 34.61,

            "additional_coverage": true,

            "policy_costs_hienfeld": 0.0,

            "contribution_guarantee_fund": 0.0

        }

    ],

    "totals": {

        "total_premium_excl_ab": 181.7,

        "total_cost_excl_ab": 184.39,

        "total_assurance_tax": 38.73,

        "total_provision_aon": 18.17,

        "total_policy_costs_hienfeld": 2.69,

        "total_insurance_cost": 223.12

    }

}

```

### POST `/dialoggroup/jagers/membership`

Verify membership existence.

  

**Request:**

```json

{

    "membership_nr": "12345"

}

```

**Response**:

```

    true


```

### GET `/dialoggroup/jagers/membership`

Get membership details. Retrieves all memberships, **Or** pas a membership number in the request parameters like this: hunter_membership_number 30989. Since it is a get request, we cannot post it as a body. The API endpoint call for the production environment would then look like this (In Postman for example):

https://rekenbox.hienfeld.io/dialoggroup/jagers/membership?hunter_membership_number=30989

  
**Response**:


```json

{

    "membership_nr": "30989",

    "association_name": "NOJG",

    "active": true

}

```



## Models

  

### LiabilityCoverage

- Stores insurance coverage details

- Calculates various costs (provision, tax, total)

- Supports base and additional coverages

  

### MembershipNumber

- Stores membership information

- Tracks active status

- Links to association name

  

## Management Commands

  

### Update NOJG Memberships

```bash

python manage.py update_nojg_memberships

```

Options:

- `--dry-run`: Test without making changes

- `--test-connection`: Verify API connectivity

- `--max-pages`: Limit number of pages to process

- `--timeout`: Set maximum runtime

  

### Populate Liability Coverage

```bash

python manage.py populate_liability_coverage

```

  

## Admin Interface

  

### Upload Membership Data

- Excel file upload support

- Automatic status updates

- Batch processing capability

  

### Manage Liability Coverage

- View and edit coverage details

- Monitor insurance rates

- Configure tax rates and fees

  

## Integration Features

- NOJG API integration for member verification

- Webhook support for membership updates

- Excel import/export capabilities

  

## Testing

Run tests with:

```bash

python manage.py test dialoggroup_hunters_2024_simpel.test_views

```

  

## Key Components

1. Premium Calculation

   - Base coverage requirements

   - Additional coverage options

   - Tax and fee calculations

  

2. Membership Management

   - Active status tracking

   - API-based synchronization

   - Manual upload support

  

3. Security

   - Token-based authentication

   - API key protection

   - Scope-based permissions

  

Remember to:

1. Keep API credentials secure

2. Regularly sync membership data

3. Monitor webhook performance

4. Update premium rates as needed

5. Test calculations thoroughly