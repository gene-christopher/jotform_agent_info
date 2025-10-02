# Agent Lookup Widget - Webhook Version

A JotForm custom widget that displays comprehensive agent information received from a Power Automate webhook. This widget is designed to work with SSO-prefilled forms and automatically displays agent details when the agent code is provided.

## Features

- **Beautiful UI**: Modern, responsive design with organized information display
- **Comprehensive Data**: Displays 17 different agent information fields
- **Real-time Updates**: Automatically updates when webhook data is received
- **JotForm Integration**: Properly integrates with JotForm's custom widget API
- **Error Handling**: Graceful loading and error states
- **Status Badges**: Visual status indicators for agent status
- **Security Features**: Multiple layers of security to prevent unauthorized access

## Security Features

The widget includes several security measures to prevent unauthorized access:

### Basic Security (agent-lookup-widget.html)
- **Referrer Validation**: Checks that requests come from JotForm domains
- **Iframe Context Validation**: Ensures widget is loaded within JotForm iframe
- **URL Parameter Validation**: Validates JotForm-specific URL parameters
- **API Availability Check**: Ensures JotForm Custom Widget API is available

### Enhanced Security (agent-lookup-widget-secure.html)
- **All Basic Security Features** plus:
- **Security Token**: Optional token-based authentication
- **Multiple Validation Layers**: Combines multiple security checks
- **Security Warning Display**: Shows security notice to users
- **Enhanced Error Handling**: More detailed security error messages

### Security Domains
The widget validates against these allowed domains:
- `jotform.com`
- `submit.jotform.com`
- `form.jotform.com`
- `www.jotform.com`

## Setup Instructions

### 1. Power Automate Flow Setup

Create a Power Automate flow with the following structure:

#### Trigger
- **Type**: "When an HTTP request is received"
- **Method**: POST
- **Request Body JSON Schema**:
```json
{
    "type": "object",
    "properties": {
        "agent_code": {
            "type": "string"
        }
    }
}
```

#### Actions
1. **HTTP Action** - Call your existing agent lookup API
   - **Method**: POST
   - **URI**: Your existing API endpoint
   - **Headers**: `Content-Type: application/json`
   - **Body**:
   ```json
   {
       "as_earned_AgentCode": "@{triggerBody()?['agent_code']}"
   }
   ```

2. **Set Variables** - Extract data from API response
   ```
   Set look_up_AgentID = @{body('HTTP')?['data']?['as_earned_AgentID']}
   Set look_up_NPN = @{body('HTTP')?['data']?['as_earned_NPN']}
   Set look_up_agent_name = @{body('HTTP')?['data']?['as_earned_AgentName']}
   Set look_up_OptID = @{body('HTTP')?['data']?['as_earned_OptID']}
   Set look_up_Status = @{body('HTTP')?['data']?['as_earned_Status']}
   Set look_up_Division = @{body('HTTP')?['data']?['as_earned_Division']}
   Set look_up_ContractStartDt = @{body('HTTP')?['data']?['as_earned_ContractStartDt']}
   Set look_up_FirstName = @{body('HTTP')?['data']?['as_earned_FirstName']}
   Set look_up_LastName = @{body('HTTP')?['data']?['as_earned_LastName']}
   Set look_up_MiddleName = @{body('HTTP')?['data']?['as_earned_MiddleName']}
   Set look_up_ContractLevel = @{body('HTTP')?['data']?['as_earned_ContractLevel']}
   Set look_up_StreetAddress = @{body('HTTP')?['data']?['as_earned_StreetAddress']}
   Set look_up_City = @{body('HTTP')?['data']?['as_earned_City']}
   Set look_up_County = @{body('HTTP')?['data']?['as_earned_County']}
   Set look_up_State = @{body('HTTP')?['data']?['as_earned_State']}
   Set look_up_Zip = @{body('HTTP')?['data']?['as_earned_Zip']}
   Set look_up_AgentPhone = @{body('HTTP')?['data']?['as_earned_AgentPhone']}
   ```

3. **Response Action** - Return formatted data
   - **Status Code**: 200
   - **Body**:
   ```json
   {
     "data": {
       "look_up_AgentID": "@{variables('look_up_AgentID')}",
       "look_up_NPN": "@{variables('look_up_NPN')}",
       "look_up_agent_name": "@{variables('look_up_agent_name')}",
       "look_up_OptID": "@{variables('look_up_OptID')}",
       "look_up_Status": "@{variables('look_up_Status')}",
       "look_up_Division": "@{variables('look_up_Division')}",
       "look_up_ContractStartDt": "@{variables('look_up_ContractStartDt')}",
       "look_up_FirstName": "@{variables('look_up_FirstName')}",
       "look_up_LastName": "@{variables('look_up_LastName')}",
       "look_up_MiddleName": "@{variables('look_up_MiddleName')}",
       "look_up_ContractLevel": "@{variables('look_up_ContractLevel')}",
       "look_up_StreetAddress": "@{variables('look_up_StreetAddress')}",
       "look_up_City": "@{variables('look_up_City')}",
       "look_up_County": "@{variables('look_up_County')}",
       "look_up_State": "@{variables('look_up_State')}",
       "look_up_Zip": "@{variables('look_up_Zip')}",
       "look_up_AgentPhone": "@{variables('look_up_AgentPhone')}"
     }
   }
   ```

### 2. JotForm Setup

1. **Upload the widget** to your GitHub repository
2. **Register the widget** in JotForm:
   - **Name**: Agent Lookup Widget
   - **Type**: iFrame Widget
   - **URL**: `https://your-username.github.io/your-repo/agent-lookup-widget.html`
   - **Width**: 800px
   - **Height**: 600px

3. **Add the widget** to your form

4. **Set up webhook** in JotForm:
   - **Trigger**: Field change on `agent_code` field
   - **Webhook URL**: Your Power Automate webhook URL
   - **Payload**: `{"agent_code": "{agent_code}"}`

### 3. Form Field Setup

1. **Create an `agent_code` field** in your JotForm
2. **Set up SSO** to populate this field automatically
3. **Add the custom widget** to display agent information
4. **Configure the webhook** to trigger when `agent_code` changes

## How It Works

1. **User logs in** → SSO populates `agent_code` field
2. **JotForm webhook triggers** → Calls Power Automate with agent code
3. **Power Automate** → Looks up agent information via your API
4. **Power Automate** → Returns formatted agent data
5. **Widget receives data** → Displays comprehensive agent information
6. **User reviews** → Sees all agent details before submitting

## Data Fields Displayed

The widget displays the following agent information:

- **Agent ID** - Unique agent identifier
- **NPN** - National Producer Number
- **Agent Name** - Full agent name
- **Opt ID** - Optional identifier
- **Status** - Agent status with color coding
- **Division** - Agent division
- **Contract Start Date** - When contract began
- **Contract Level** - Level of contract
- **Name Details** - First, Last, Middle names
- **Address** - Street, City, County, State, ZIP
- **Phone** - Agent phone number

## Customization

The widget can be customized by modifying the CSS styles in the `<style>` section:

- **Colors**: Update CSS variables for different color schemes
- **Layout**: Modify grid layout and spacing
- **Fields**: Add or remove information fields
- **Styling**: Update fonts, borders, and visual elements

## Browser Support

- Chrome 60+
- Firefox 55+
- Safari 12+
- Edge 79+

## License

This project is open source and available under the MIT License.
