# Requirements Document: Ferri-Wala

## Introduction

Ferri-Wala is a dual-mode mobility application that connects street hawkers with potential buyers through real-time location tracking and intelligent request management. The system enables buyers to discover nearby hawkers and their products, while providing hawkers with optimized routing based on buyer demand. The application supports both web (ReactJs) and mobile (React Native) platforms with a unified JavaScript codebase.

## Glossary

- **Buyer**: A user seeking to purchase products from street hawkers
- **Hawker**: A mobile vendor who sells products while moving through neighborhoods
- **Buyer_App**: The application interface used by buyers to find hawkers and make requests
- **Hawker_App**: The application interface used by hawkers to share location and view requests
- **Location_Service**: The system component that tracks and shares real-time GPS coordinates
- **Request**: A buyer's notification to a hawker expressing interest in purchasing specific products
- **Route_Optimizer**: The AI-powered system that calculates optimal paths for hawkers
- **Map_Interface**: The visual map component displaying locations and requests
- **Time_Range**: A buyer-specified window during which they are available for purchase
- **Commodity**: A product or item sold by hawkers

## Requirements

### Requirement 1: User Mode Selection

**User Story:** As a user, I want to choose between Buyer mode and Hawker mode when launching the app, so that I can access the appropriate features for my role.

#### Acceptance Criteria

1. WHEN the application starts, THE Buyer_App SHALL display a mode selection interface
2. WHEN a user selects Buyer mode, THE Buyer_App SHALL activate buyer-specific features and interface
3. WHEN a user selects Hawker mode, THE Hawker_App SHALL activate hawker-specific features and interface
4. THE Buyer_App SHALL persist the selected mode for subsequent sessions
5. THE Buyer_App SHALL provide an option to switch modes without restarting the application

### Requirement 2: Buyer Location Discovery

**User Story:** As a buyer, I want to see nearby hawkers on a map with their product listings, so that I can find vendors selling items I need.

#### Acceptance Criteria

1. WHEN a buyer opens the map view, THE Map_Interface SHALL display all active hawkers within a 5-kilometer radius
2. WHEN displaying hawker locations, THE Map_Interface SHALL show each hawker's current GPS coordinates as a map marker
3. WHEN a buyer taps a hawker marker, THE Buyer_App SHALL display the list of commodities sold by that hawker
4. WHILE the map is active, THE Location_Service SHALL update hawker positions every 10 seconds
5. WHEN no hawkers are nearby, THE Buyer_App SHALL display a message indicating no active hawkers in the area

### Requirement 3: Buyer Request Creation

**User Story:** As a buyer, I want to send purchase requests to specific hawkers with time preferences, so that hawkers know I'm interested in buying their products.

#### Acceptance Criteria

1. WHEN a buyer selects a hawker and commodity, THE Buyer_App SHALL provide an interface to create a purchase request
2. WHEN creating a request, THE Buyer_App SHALL require the buyer to specify at least one commodity
3. WHEN creating a request, THE Buyer_App SHALL allow the buyer to specify a time range for availability
4. WHEN a buyer submits a request, THE Buyer_App SHALL transmit the request to the selected hawker immediately
5. WHEN a request is submitted, THE Buyer_App SHALL include the buyer's current location with the request
6. IF network connectivity is unavailable, THEN THE Buyer_App SHALL queue the request and retry transmission when connectivity is restored

### Requirement 4: Hawker Location Broadcasting

**User Story:** As a hawker, I want to share my real-time location with buyers, so that they can find me and make purchase requests.

#### Acceptance Criteria

1. WHEN a hawker activates location sharing, THE Location_Service SHALL broadcast the hawker's GPS coordinates every 5 seconds
2. WHILE location sharing is active, THE Hawker_App SHALL display a visual indicator confirming broadcast status
3. WHEN a hawker deactivates location sharing, THE Location_Service SHALL stop broadcasting coordinates and remove the hawker from buyer maps
4. THE Hawker_App SHALL request location permissions before activating location sharing
5. IF location permissions are denied, THEN THE Hawker_App SHALL display an error message and prevent activation of hawker mode

### Requirement 5: Hawker Request Visualization

**User Story:** As a hawker, I want to see buyer requests on a map, so that I can identify where demand exists and plan my route accordingly.

#### Acceptance Criteria

1. WHEN a hawker opens the map view, THE Map_Interface SHALL display all active buyer requests within a 5-kilometer radius
2. WHEN displaying requests, THE Map_Interface SHALL show each request's location as a map marker
3. WHEN a hawker taps a request marker, THE Hawker_App SHALL display the requested commodity and time range
4. WHILE the map is active, THE Hawker_App SHALL update request locations every 10 seconds
5. WHEN a request's time range expires, THE Map_Interface SHALL remove that request from the map

### Requirement 6: AI-Powered Route Optimization

**User Story:** As a hawker, I want the system to calculate the most efficient route through buyer requests, so that I can maximize sales while minimizing travel time.

#### Acceptance Criteria

1. WHEN a hawker requests route optimization, THE Route_Optimizer SHALL analyze all visible buyer requests
2. WHEN calculating routes, THE Route_Optimizer SHALL consider request time ranges and current time
3. WHEN calculating routes, THE Route_Optimizer SHALL prioritize requests with earlier expiration times
4. WHEN a route is calculated, THE Map_Interface SHALL display the suggested path on the map
5. WHEN displaying the route, THE Hawker_App SHALL show estimated travel time for each segment
6. THE Route_Optimizer SHALL recalculate the route when new requests appear or existing requests expire

### Requirement 7: Cross-Platform Compatibility

**User Story:** As a user, I want to access Ferri-Wala on both web browsers and mobile devices, so that I can use the app on my preferred platform.

#### Acceptance Criteria

1. THE Buyer_App SHALL function on web browsers using ReactJs
2. THE Buyer_App SHALL function on iOS and Android devices using React Native
3. WHEN accessing the web version, THE Buyer_App SHALL provide a responsive layout for desktop and mobile browsers
4. WHEN switching between platforms, THE Buyer_App SHALL maintain user session and preferences
5. THE Buyer_App SHALL implement all core features identically across web and mobile platforms

### Requirement 8: Visual Design Consistency

**User Story:** As a user, I want a consistent and pleasant visual experience, so that the app is easy to use and aesthetically appealing.

#### Acceptance Criteria

1. THE Buyer_App SHALL use only the following color palette: #c4b8d4, #c9def2, #c5d4d0, #dbd0d6
2. THE Map_Interface SHALL use distinct colors from the palette for buyer markers, hawker markers, and routes
3. WHEN displaying UI components, THE Buyer_App SHALL maintain consistent spacing and typography across all screens
4. THE Buyer_App SHALL provide sufficient color contrast for text readability
5. WHEN rendering on different screen sizes, THE Buyer_App SHALL adapt layouts while maintaining visual consistency

### Requirement 9: Real-Time Data Synchronization

**User Story:** As a user, I want location and request data to update in real-time, so that I always see current information.

#### Acceptance Criteria

1. WHEN a hawker's location changes, THE Location_Service SHALL propagate the update to all nearby buyers within 10 seconds
2. WHEN a buyer creates a request, THE Location_Service SHALL deliver it to the target hawker within 5 seconds
3. WHEN network latency exceeds 30 seconds, THE Buyer_App SHALL display a connectivity warning
4. THE Location_Service SHALL use WebSocket connections for real-time updates on web platforms
5. THE Location_Service SHALL use native push notifications for real-time updates on mobile platforms

### Requirement 10: Location Privacy and Control

**User Story:** As a user, I want control over my location sharing, so that I can protect my privacy when not actively using the app.

#### Acceptance Criteria

1. THE Buyer_App SHALL only access location data when the user has granted explicit permission
2. WHEN a buyer closes the app, THE Location_Service SHALL stop broadcasting the buyer's location
3. WHEN a hawker switches to Buyer mode, THE Location_Service SHALL stop broadcasting the hawker's location
4. THE Buyer_App SHALL provide a settings interface to revoke location permissions
5. WHEN location permissions are revoked, THE Buyer_App SHALL display a message explaining which features are unavailable
