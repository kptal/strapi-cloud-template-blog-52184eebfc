# How to Fix 403 Forbidden Error in Strapi

## The Issue
You're getting a 403 Forbidden error when trying to access the API endpoints. This is a permissions issue in Strapi.

## Solution

### Step 1: Access Strapi Admin Panel
1. Start your Strapi server: `npm run develop`
2. Open http://localhost:1337/admin
3. Log in with your admin credentials

### Step 2: Enable Public Access for Content Types

For each content type (Teams, Stadiums, Boards), you need to enable public access:

1. Go to **Settings** → **Users & Permissions Plugin** → **Roles**
2. Click on the **Public** role
3. Scroll down to find your content types:
   - **Team**
   - **Stadium**
   - **Board**

### Step 3: Grant Permissions

For each content type, check the following permissions:
- ✅ **find** - List all items
- ✅ **findOne** - Get single item
- ✅ **create** - Create new item (optional, only if needed)
- ✅ **update** - Update item (optional, only if needed)
- ✅ **delete** - Delete item (optional, only if needed)

### Step 4: Save Changes
Click the **Save** button at the bottom

### Step 5: Test the API

Try these curl commands to verify access:

```bash
# Test Teams
curl -X GET "http://localhost:1337/api/teams"

# Test Stadiums
curl -X GET "http://localhost:1337/api/stadiums"

# Test Boards
curl -X GET "http://localhost:1337/api/boards"
```

## Alternative: Using Environment Variables

You can also configure permissions programmatically by creating an initial role setup in your seed script.

## For Development Only

If you want to enable all permissions for development:

1. Go to **Settings** → **Users & Permissions Plugin** → **Roles**
2. Click **Public**
3. Check **Select All** button for your content types
4. Save

**Note:** This is for development only. For production, be more selective with permissions.

## Advanced: Custom Role Configuration

If you need authenticated users with different permissions:

1. Create a custom role in **Roles**
2. Assign specific permissions to that role
3. Create users and assign them to that role
4. Authenticate users and use JWT tokens

### Example: Authenticated Request
```bash
curl -X GET "http://localhost:1337/api/teams" \
  -H "Authorization: Bearer YOUR_JWT_TOKEN"
```

## Troubleshooting

### Still getting 403 after enabling permissions?

1. **Clear browser cache** - Strapi caches permissions
2. **Restart server** - Run `npm run develop` again
3. **Check user role** - Make sure your user has the right role assigned
4. **Verify API token** - If using API tokens, ensure the token has the right permissions

### Need to create an API Token instead?

1. Go to **Settings** → **API Tokens**
2. Click **Create new API token**
3. Give it a name and select permissions for each content type
4. Use the token in your requests:

```bash
curl -X GET "http://localhost:1337/api/teams" \
  -H "Authorization: Bearer YOUR_API_TOKEN"
```

## API Endpoints that need permissions enabled

### Teams
- `GET /api/teams` - List teams
- `GET /api/teams/{id}` - Get single team
- `POST /api/teams` - Create team (optional)
- `PUT /api/teams/{id}` - Update team (optional)
- `DELETE /api/teams/{id}` - Delete team (optional)

### Stadiums
- `GET /api/stadiums` - List stadiums
- `GET /api/stadiums/{id}` - Get single stadium
- `POST /api/stadiums` - Create stadium (optional)
- `PUT /api/stadiums/{id}` - Update stadium (optional)
- `DELETE /api/stadiums/{id}` - Delete stadium (optional)

### Boards
- `GET /api/boards` - List boards
- `GET /api/boards/{id}` - Get single board
- `POST /api/boards` - Create board (optional)
- `PUT /api/boards/{id}` - Update board (optional)
- `DELETE /api/boards/{id}` - Delete board (optional)

## Quick Reference

The permission levels in Strapi are:
- **Public** - Anyone can access (no authentication needed)
- **Authenticated** - Only logged-in users
- **Custom Roles** - Users with specific roles

For a public API with read access, enable **Public** role permissions for `find` and `findOne` actions.
