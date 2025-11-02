# APPENDIX

## A. LIST OF ABBREVIATIONS

| Abbreviation | Full Form |
|--------------|-----------|
| API | Application Programming Interface |
| BaaS | Backend-as-a-Service |
| CDN | Content Delivery Network |
| CMS | Content Management System |
| CSP | Content Security Policy |
| CRUD | Create, Read, Update, Delete |
| CSS | Cascading Style Sheets |
| DBMS | Database Management System |
| DOM | Document Object Model |
| ES6 | ECMAScript 6 (JavaScript standard) |
| FCP | First Contentful Paint |
| FID | First Input Delay |
| GPS | Global Positioning System |
| HTML | HyperText Markup Language |
| HTTP | HyperText Transfer Protocol |
| HTTPS | HyperText Transfer Protocol Secure |
| HMR | Hot Module Replacement |
| ICT | Information and Communication Technology |
| IoT | Internet of Things |
| i18n | Internationalization |
| JSON | JavaScript Object Notation |
| JSX | JavaScript XML |
| LCP | Largest Contentful Paint |
| PWA | Progressive Web App |
| QA | Quality Assurance |
| REST | Representational State Transfer |
| RTL | Right-to-Left |
| SDLC | Software Development Life Cycle |
| SPA | Single Page Application |
| SQL | Structured Query Language |
| TLS | Transport Layer Security |
| TTI | Time to Interactive |
| UI | User Interface |
| UX | User Experience |
| WCAG | Web Content Accessibility Guidelines |
| XSS | Cross-Site Scripting |
| CSRF | Cross-Site Request Forgery |

## B. SCREENSHOTS

### B.1 Application Home Page

*[Screenshot of AgriSmart home page showing:*
- *Application header with logo and title*
- *Crop selection dropdown interface*
- *Initial empty state before crop selection*
- *Responsive layout on mobile/desktop view]*

**Description:** The home page displays the crop selection interface, allowing users to choose from available crops. The interface is clean and intuitive, with clear visual hierarchy guiding users to the primary action.

### B.2 Crop Selection Interface

*[Screenshot showing:*
- *Dropdown menu with list of available crops*
- *Search functionality in action*
- *Selected crop highlighted*
- *Touch-friendly interface on mobile device]*

**Description:** The crop selection interface enables users to search and select crops from the database. The interface supports both keyboard and touch input, ensuring accessibility across devices.

### B.3 Crop Information Display - Mobile View

*[Screenshot of crop information displayed on mobile device showing:*
- *Crop name and header*
- *Sowing schedule section with calendar icon*
- *Irrigation schedule section*
- *Growth stages information*
- *Harvesting information*
- *Mobile-optimized layout with proper spacing]*

**Description:** The mobile view demonstrates the responsive design, with information organized vertically for optimal mobile viewing. Touch targets are appropriately sized for mobile interaction.

### B.4 Crop Information Display - Desktop View

*[Screenshot of crop information displayed on desktop showing:*
- *Wider layout with optimized spacing*
- *Multiple sections visible simultaneously*
- *Enhanced visual hierarchy*
- *Desktop-optimized typography and layout]*

**Description:** The desktop view showcases the responsive design adapting to larger screens, with improved spacing and layout optimization for desktop viewing.

### B.5 Sowing Schedule Section

*[Screenshot focusing on sowing schedule component showing:*
- *Season information*
- *Optimal sowing months*
- *Temperature requirements*
- *Visual icons and clear formatting]*

**Description:** Detailed view of the sowing schedule section, demonstrating clear information presentation with appropriate visual elements.

### B.6 Irrigation Schedule Section

*[Screenshot showing irrigation information:*
- *Irrigation frequency*
- *Stage-based irrigation schedule*
- *Critical stages highlighted*
- *Water requirements]*

**Description:** The irrigation schedule section displays comprehensive irrigation guidance with stage-specific recommendations.

### B.7 Dark Mode View

*[Screenshot showing application in dark mode:*
- *Dark background*
- *Light text for contrast*
- *Color scheme adjusted for dark mode*
- *Maintained readability]*

**Description:** Dark mode implementation demonstrating color adjustments for optimal viewing in low-light conditions.

## C. CODE SNIPPETS

### C.1 Application Entry Point (main.jsx)

```javascript
import { StrictMode } from 'react'
import { createRoot } from 'react-dom/client'
import './index.css'
import App from './App.jsx'

createRoot(document.getElementById('root')).render(
  <StrictMode>
    <App />
  </StrictMode>,
)
```

**Description:** Entry point of the React application, initializing the root and rendering the App component with React StrictMode enabled for development warnings.

### C.2 Root App Component (App.jsx)

```javascript
import { useState, useEffect } from 'react'
import Header from './components/Header'
import CropSelector from './components/CropSelector'
import CropInfo from './components/CropInfo'
import cropsData from './data/crops.json'

function App() {
  const [selectedCrop, setSelectedCrop] = useState(null)
  const [cropData, setCropData] = useState(null)
  const [loading, setLoading] = useState(false)
  const [error, setError] = useState(null)

  useEffect(() => {
    if (selectedCrop) {
      setLoading(true)
      setError(null)
      
      const crop = cropsData.crops.find(c => c.id === selectedCrop)
      
      if (crop) {
        setCropData(crop)
        setLoading(false)
      } else {
        setError('Crop not found')
        setLoading(false)
      }
    } else {
      setCropData(null)
    }
  }, [selectedCrop])

  const handleCropSelect = (cropId) => {
    setSelectedCrop(cropId)
  }

  return (
    <div className="min-h-screen bg-white dark:bg-gray-950">
      <Header />
      <main className="container mx-auto px-4 py-8">
        <CropSelector 
          crops={cropsData.crops} 
          onCropSelect={handleCropSelect}
          selectedCrop={selectedCrop}
        />
        {loading && <div>Loading...</div>}
        {error && <div className="text-red-600">Error: {error}</div>}
        {cropData && <CropInfo cropData={cropData} />}
      </main>
    </div>
  )
}

export default App
```

**Description:** Main application component managing global state, coordinating data retrieval, and rendering child components based on application state.

### C.3 Crop Selector Component

```javascript
import { useState } from 'react'
import { Search } from 'lucide-react'

function CropSelector({ crops, onCropSelect, selectedCrop }) {
  const [searchTerm, setSearchTerm] = useState('')
  const [isOpen, setIsOpen] = useState(false)

  const filteredCrops = crops.filter(crop =>
    crop.name.toLowerCase().includes(searchTerm.toLowerCase())
  )

  const handleSelect = (cropId) => {
    onCropSelect(cropId)
    setIsOpen(false)
    setSearchTerm('')
  }

  return (
    <div className="relative w-full max-w-md mx-auto mb-8">
      <label className="block text-lg font-semibold mb-2 text-gray-700 dark:text-gray-300">
        Select a Crop
      </label>
      <div className="relative">
        <input
          type="text"
          value={searchTerm}
          onChange={(e) => setSearchTerm(e.target.value)}
          onFocus={() => setIsOpen(true)}
          placeholder="Search for a crop..."
          className="w-full px-4 py-3 pl-10 border border-gray-300 rounded-lg focus:ring-2 focus:ring-green-500 focus:border-transparent"
        />
        <Search className="absolute left-3 top-3.5 h-5 w-5 text-gray-400" />
      </div>
      
      {isOpen && filteredCrops.length > 0 && (
        <div className="absolute z-10 w-full mt-1 bg-white border border-gray-300 rounded-lg shadow-lg max-h-60 overflow-auto">
          {filteredCrops.map(crop => (
            <button
              key={crop.id}
              onClick={() => handleSelect(crop.id)}
              className={`w-full text-left px-4 py-2 hover:bg-green-50 ${
                selectedCrop === crop.id ? 'bg-green-100' : ''
              }`}
            >
              {crop.name}
            </button>
          ))}
        </div>
      )}
    </div>
  )
}

export default CropSelector
```

**Description:** Crop selection component with search functionality, dropdown interface, and user interaction handling.

### C.4 Vite Configuration (vite.config.js)

```javascript
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'

export default defineConfig({
  plugins: [react()],
  build: {
    outDir: 'dist',
    sourcemap: true,
    rollupOptions: {
      output: {
        manualChunks: {
          vendor: ['react', 'react-dom'],
          router: ['react-router-dom']
        }
      }
    }
  }
})
```

**Description:** Vite build configuration specifying React plugin, build output directory, source maps, and code splitting strategy.

### C.5 Tailwind CSS Configuration (tailwind.config.cjs)

```javascript
/** @type {import('tailwindcss').Config} */
module.exports = {
  darkMode: 'class',
  content: [
    './index.html',
    './src/**/*.{js,jsx,ts,tsx}',
  ],
  theme: {
    extend: {
      colors: {
        primary: {
          DEFAULT: '#16a34a',
          50: '#ecfdf5',
          100: '#d1fae5',
          200: '#a7f3d0',
          300: '#6ee7b7',
          400: '#34d399',
          500: '#10b981',
          600: '#16a34a',
          700: '#15803d',
          800: '#166534',
          900: '#14532d'
        },
        soil: '#8b5e3c',
        sky: '#38bdf8'
      }
    },
  },
  plugins: [],
}
```

**Description:** Tailwind CSS configuration with custom color palette, dark mode support, and content paths for purging unused styles.

### C.6 Package.json Scripts

```json
{
  "scripts": {
    "dev": "vite",
    "build": "vite build",
    "preview": "vite preview",
    "format": "prettier --write ."
  }
}
```

**Description:** NPM scripts for development server, production build, preview, and code formatting.

## D. CROP DATABASE SCHEMA

### D.1 Complete JSON Schema Structure

```json
{
  "crops": [
    {
      "id": 1,
      "name": "Wheat",
      "category": "Cereals",
      "commonNames": ["गहू", "गेहूं"],
      "sowing": {
        "season": "Rabi",
        "optimalMonths": ["October", "November"],
        "temperature": "15-25°C",
        "soilTemperature": "15-20°C",
        "description": "Wheat is best sown in Rabi season when temperatures are moderate."
      },
      "irrigation": {
        "frequency": "Weekly during active growth",
        "criticalStages": [
          {
            "stage": "Crown Root Initiation",
            "timing": "21-25 days after sowing",
            "frequency": "Every 7-10 days",
            "importance": "Critical for root development"
          },
          {
            "stage": "Tillering",
            "timing": "30-45 days after sowing",
            "frequency": "Every 10-12 days",
            "importance": "Essential for yield formation"
          }
        ],
        "totalIrrigations": "4-6 irrigations",
        "waterRequirement": "400-500mm"
      },
      "growthStages": [
        {
          "stage": "Germination",
          "duration": "7-10 days",
          "description": "Seed absorbs water and begins to sprout"
        },
        {
          "stage": "Seedling",
          "duration": "10-15 days",
          "description": "First leaves emerge, root system develops"
        }
      ],
      "harvesting": {
        "duration": "120-150 days from sowing",
        "optimalMonths": ["March", "April"],
        "indicators": [
          "Grains hard and golden in color",
          "Moisture content 20-25%",
          "Stems turn yellow and dry"
        ],
        "methods": [
          "Manual harvesting with sickle",
          "Mechanical harvesting with combine harvester"
        ],
        "storage": "Store in dry place with moisture content below 12%"
      }
    }
  ]
}
```

### D.2 Field Descriptions

**Root Level:**
- `crops`: Array of crop objects

**Crop Object Fields:**
- `id`: Integer - Unique identifier for the crop
- `name`: String - Primary name of the crop (required)
- `category`: String - Crop category (e.g., "Cereals", "Pulses") - optional
- `commonNames`: Array of Strings - Alternative names or local language names - optional

**Sowing Object:**
- `season`: String - Agricultural season (Rabi, Kharif, Zaid)
- `optimalMonths`: Array of Strings - Best months for sowing
- `temperature`: String - Optimal temperature range
- `soilTemperature`: String - Optimal soil temperature - optional
- `description`: String - Additional sowing information - optional

**Irrigation Object:**
- `frequency`: String - General irrigation frequency
- `criticalStages`: Array of Objects - Stage-specific irrigation requirements
  - `stage`: String - Name of growth stage
  - `timing`: String - When this stage occurs
  - `frequency`: String - Irrigation frequency for this stage
  - `importance`: String - Importance level - optional
- `totalIrrigations`: String - Total number of irrigations required
- `waterRequirement`: String - Total water requirement - optional

**Growth Stages Array:**
- `stage`: String - Name of growth stage
- `duration`: String - Duration of this stage
- `description`: String - Description of stage characteristics

**Harvesting Object:**
- `duration`: String - Time from sowing to harvest
- `optimalMonths`: Array of Strings - Best months for harvesting
- `indicators`: Array of Strings - Signs indicating readiness for harvest
- `methods`: Array of Strings - Harvesting methods - optional
- `storage`: String - Storage recommendations - optional

### D.3 Example Crop Entries

**Example 1: Rice (Kharif Crop)**

```json
{
  "id": 2,
  "name": "Rice",
  "category": "Cereals",
  "sowing": {
    "season": "Kharif",
    "optimalMonths": ["June", "July"],
    "temperature": "25-35°C",
    "description": "Rice requires warm temperatures and abundant water."
  },
  "irrigation": {
    "frequency": "Continuous flooding or intermittent",
    "criticalStages": [
      {
        "stage": "Transplanting",
        "frequency": "Immediate after transplanting"
      },
      {
        "stage": "Tillering",
        "frequency": "Maintain 5-7 cm water depth"
      }
    ],
    "totalIrrigations": "Continuous water supply required",
    "waterRequirement": "1000-1500mm"
  },
  "growthStages": [
    {
      "stage": "Seedling",
      "duration": "15-20 days",
      "description": "Seedlings grow in nursery before transplanting"
    },
    {
      "stage": "Transplanting",
      "duration": "20-25 days",
      "description": "Seedlings transplanted to main field"
    }
  ],
  "harvesting": {
    "duration": "120-150 days from sowing",
    "optimalMonths": ["October", "November"],
    "indicators": [
      "Grains hard and yellow",
      "Moisture content 20-22%",
      "80% of grains mature"
    ]
  }
}
```

**Example 2: Cotton (Kharif Crop)**

```json
{
  "id": 3,
  "name": "Cotton",
  "category": "Cash Crops",
  "sowing": {
    "season": "Kharif",
    "optimalMonths": ["April", "May", "June"],
    "temperature": "25-30°C",
    "soilTemperature": "18-20°C"
  },
  "irrigation": {
    "frequency": "Every 10-15 days",
    "criticalStages": [
      {
        "stage": "Flowering",
        "timing": "60-90 days after sowing",
        "frequency": "Every 10 days",
        "importance": "Critical for boll development"
      },
      {
        "stage": "Boll Formation",
        "timing": "90-120 days",
        "frequency": "Every 12-15 days"
      }
    ],
    "totalIrrigations": "8-10 irrigations",
    "waterRequirement": "600-800mm"
  },
  "harvesting": {
    "duration": "150-180 days from sowing",
    "optimalMonths": ["October", "November", "December"],
    "indicators": [
      "Bolls fully opened",
      "Lint completely dried",
      "Seed cotton ready for picking"
    ],
    "methods": [
      "Manual picking",
      "Machine picking"
    ]
  }
}
```

## E. USER MANUAL

### E.1 Getting Started

**Accessing AgriSmart:**

1. **Open Web Browser:**
   - AgriSmart works in any modern web browser (Chrome, Firefox, Safari, Edge)
   - No installation required - simply navigate to the application URL

2. **Device Compatibility:**
   - Works on smartphones, tablets, and desktop computers
   - Optimized for mobile devices with touch-friendly interface

3. **First Visit:**
   - The application loads automatically
   - You'll see the crop selection interface immediately
   - No registration or login required

### E.2 Selecting a Crop

**Step-by-Step Instructions:**

1. **Locate Crop Selector:**
   - Find the "Select a Crop" dropdown at the top of the page
   - The dropdown displays all available crops

2. **Search for a Crop (Optional):**
   - Click on the search field
   - Type the name of the crop you're looking for
   - The list will filter as you type, showing matching crops

3. **Select a Crop:**
   - Click on the crop name from the dropdown list
   - The selected crop will be highlighted
   - Crop information will appear below

**Tips:**
- You can type part of a crop name to find it quickly
- The search is case-insensitive
- Use the search feature if you're unsure of exact crop name

### E.3 Viewing Crop Information

**Information Sections:**

Once you select a crop, the following information sections will appear:

**1. Sowing Schedule:**
- **Season:** The agricultural season (Rabi, Kharif, or Zaid)
- **Optimal Months:** Best months for sowing the crop
- **Temperature:** Required temperature range for sowing
- **Additional Details:** Any specific sowing requirements

**2. Irrigation Schedule:**
- **Frequency:** How often to irrigate
- **Critical Stages:** Important growth stages requiring specific irrigation
- **Total Irrigations:** Overall irrigation requirements
- **Water Requirements:** Total water needed for the crop

**3. Growth Stages:**
- **Stage Names:** Different phases of crop growth
- **Duration:** Time period for each stage
- **Descriptions:** What happens during each stage

**4. Harvesting Information:**
- **Duration:** Total time from sowing to harvest
- **Optimal Months:** Best months for harvesting
- **Indicators:** Signs that the crop is ready for harvest
- **Methods:** How to harvest the crop

### E.4 Navigation

**Selecting Another Crop:**
- Simply click on the crop selector again
- Choose a different crop from the dropdown
- Previous crop information will be replaced with new crop data

**Mobile Navigation:**
- Scroll down to view all information sections
- Tap on sections to expand (if applicable)
- Use touch gestures to navigate smoothly

**Desktop Navigation:**
- Scroll with mouse or keyboard
- Use keyboard shortcuts if implemented
- Click on different sections as needed

### E.5 Troubleshooting

**Common Issues and Solutions:**

**Issue: Crop information not appearing**
- **Solution:** Make sure you've selected a crop from the dropdown
- **Solution:** Check your internet connection
- **Solution:** Refresh the page and try again

**Issue: Can't find a specific crop**
- **Solution:** Use the search function to find crops by partial name
- **Solution:** Check if the crop is available in the database
- **Solution:** Try alternative crop names or common names

**Issue: Information not loading**
- **Solution:** Check your internet connection
- **Solution:** Wait a few seconds for the page to load
- **Solution:** Refresh the browser page

**Issue: Display looks incorrect on mobile**
- **Solution:** Make sure your browser is up to date
- **Solution:** Try rotating your device (portrait/landscape)
- **Solution:** Clear browser cache and reload

### E.6 Best Practices

**Using AgriSmart Effectively:**

1. **Check Information Regularly:**
   - Refer to crop schedules throughout the growing season
   - Use the information for planning agricultural activities

2. **Note Important Dates:**
   - Make note of optimal sowing and harvesting months
   - Plan irrigation schedules based on growth stages

3. **Combine with Local Knowledge:**
   - Use AgriSmart information as a guide
   - Adjust recommendations based on local conditions
   - Consult local agricultural experts when needed

4. **Use Mobile Device:**
   - Access AgriSmart on your smartphone for field reference
   - Bookmark the application for quick access
   - Check information before making farming decisions

## F. GLOSSARY

### F.1 Technical Terms

**Application Programming Interface (API):** A set of protocols and tools for building software applications, allowing different applications to communicate.

**Component:** A reusable piece of code that represents a part of the user interface in React.js.

**CSS (Cascading Style Sheets):** A language used to describe the presentation of web pages, including layout, colors, and fonts.

**Database:** An organized collection of data stored and accessed electronically.

**Frontend:** The part of a web application that users interact with directly, including the user interface and user experience.

**Git:** A version control system used for tracking changes in source code during software development.

**HTML (HyperText Markup Language):** The standard markup language for creating web pages.

**JavaScript:** A programming language used to create interactive effects within web browsers.

**JSON (JavaScript Object Notation):** A lightweight data-interchange format that is easy for humans to read and write and for machines to parse.

**Node.js:** A JavaScript runtime environment that executes JavaScript code outside of a web browser.

**Progressive Web App (PWA):** A web application that uses modern web capabilities to provide an app-like experience to users.

**React.js:** A JavaScript library for building user interfaces, particularly web applications.

**Responsive Design:** A web design approach that makes web pages render well on various devices and screen sizes.

**Single Page Application (SPA):** A web application that loads a single HTML page and dynamically updates content as the user interacts with the app.

**Tailwind CSS:** A utility-first CSS framework for rapidly building custom user interfaces.

**Vite:** A build tool that provides a faster and leaner development experience for modern web projects.

### F.2 Agricultural Terms

**Crop Lifecycle:** The complete process of crop development from sowing to harvesting, including all growth stages.

**Harvesting:** The process of gathering mature crops from the fields.

**Irrigation:** The artificial application of water to land for assisting in the production of crops.

**Kharif Season:** The monsoon season crop in India, typically sown in June-July and harvested in October-November.

**Rabi Season:** The winter season crop in India, typically sown in October-November and harvested in March-April.

**Sowing:** The process of planting seeds in the soil to begin crop cultivation.

**Growth Stage:** A specific phase in the development of a crop, each with distinct characteristics and requirements.

**Critical Stage:** An important phase in crop growth that requires specific attention or inputs for optimal development.

**Optimal Temperature:** The ideal temperature range for a specific agricultural activity (sowing, growth, etc.).

**Soil Temperature:** The temperature of the soil at the depth where seeds are planted or roots grow.

**Water Requirement:** The total amount of water needed for a crop throughout its lifecycle.

**Harvesting Indicators:** Physical signs or characteristics that indicate a crop is ready for harvest.

**Moisture Content:** The amount of water present in crop produce, often expressed as a percentage.

**Agricultural Season:** A specific time period during the year when certain crops are typically grown, based on climatic conditions.

**Crop Rotation:** The practice of growing different crops sequentially on the same plot of land to improve soil health and crop yields.

**Extension Service:** Agricultural advisory services provided to farmers by government or private organizations.

### F.3 User Interface Terms

**Dropdown Menu:** A list of options that appears when a user clicks on a menu button or input field.

**Touch Target:** The area on a screen that responds to touch input, typically buttons or interactive elements.

**Responsive Breakpoint:** Specific screen widths at which the layout of a website changes to accommodate different device sizes.

**Dark Mode:** A display setting that uses a dark color scheme, typically with light text on dark backgrounds.

**Mobile-First Design:** A design approach that starts with designing for mobile devices and then scales up for larger screens.

**User Experience (UX):** The overall experience a user has when interacting with a product or system.

**User Interface (UI):** The visual elements and layout that users interact with in an application.

## G. PROJECT STRUCTURE DIAGRAM

```
agrismart/
│
├── public/                          # Public static assets
│   └── index.html                   # HTML entry point
│
├── src/                             # Source code directory
│   │
│   ├── components/                  # React components
│   │   ├── Header.jsx              # Application header
│   │   ├── CropSelector.jsx        # Crop selection component
│   │   ├── CropInfo.jsx            # Crop information container
│   │   ├── SowingSchedule.jsx      # Sowing information display
│   │   ├── IrrigationSchedule.jsx  # Irrigation information display
│   │   ├── GrowthStages.jsx        # Growth stages display
│   │   └── HarvestingInfo.jsx      # Harvesting information display
│   │
│   ├── data/                        # Data files
│   │   └── crops.json              # Crop database (JSON format)
│   │
│   ├── styles/                      # Style files
│   │   └── index.css               # Global styles and Tailwind imports
│   │
│   ├── utils/                       # Utility functions
│   │   └── cropData.js             # Crop data helper functions
│   │
│   ├── App.jsx                      # Root application component
│   └── main.jsx                     # Application entry point
│
├── node_modules/                    # Dependencies (auto-generated)
│
├── dist/                            # Production build output (generated)
│
├── .gitignore                       # Git ignore patterns
├── package.json                     # Project dependencies and scripts
├── package-lock.json                # Dependency lock file
├── vite.config.js                   # Vite build configuration
├── tailwind.config.cjs              # Tailwind CSS configuration
├── postcss.config.cjs               # PostCSS configuration
└── README.md                        # Project documentation
```

## H. TECHNOLOGY VERSIONS

### H.1 Core Dependencies

| Package | Version | Purpose |
|---------|---------|---------|
| react | 18.3.1 | UI framework |
| react-dom | 18.3.1 | React DOM rendering |
| react-router-dom | 6.28.1 | Client-side routing |
| @supabase/supabase-js | 2.77.0 | Backend services |

### H.2 Development Dependencies

| Package | Version | Purpose |
|---------|---------|---------|
| vite | 7.1.12 | Build tool and dev server |
| @vitejs/plugin-react | 4.3.4 | Vite React plugin |
| tailwindcss | 3.4.18 | CSS framework |
| postcss | 8.4.49 | CSS processing |
| autoprefixer | 10.4.20 | CSS vendor prefixing |
| prettier | 3.3.3 | Code formatting |

### H.3 UI Libraries

| Package | Version | Purpose |
|---------|---------|---------|
| framer-motion | 11.3.31 | Animation library |
| lucide-react | 0.460.0 | Icon library |
| recharts | 2.13.2 | Charting library |

## I. SYSTEM REQUIREMENTS SUMMARY

### I.1 Development Requirements

**Operating System:**
- macOS 10.14+, Windows 10+, or Linux (any modern distribution)

**Software:**
- Node.js 20.19+ or 22.12+
- npm 10.8+ (included with Node.js)
- Git (for version control)
- Code Editor (VS Code recommended)

**Hardware:**
- 4GB RAM minimum (8GB recommended)
- 10GB free disk space
- Internet connection for package installation

### I.2 User Requirements

**Devices:**
- Smartphone (iOS 12+ or Android 8.0+)
- Tablet (iOS 12+ or Android 8.0+)
- Desktop/Laptop with modern browser

**Browsers:**
- Chrome (latest 2 versions)
- Firefox (latest 2 versions)
- Safari (latest 2 versions)
- Edge (latest 2 versions)

**Network:**
- Internet connection (3G or better recommended)
- No special network configuration required

**Storage:**
- 50MB free space (for browser cache)

## J. PERFORMANCE BENCHMARKS

### J.1 Target Performance Metrics

| Metric | Target | Measurement Method |
|--------|--------|-------------------|
| Initial Load Time | < 3 seconds | On 3G connection (1.6 Mbps) |
| Time to Interactive | < 3 seconds | Lighthouse TTI metric |
| First Contentful Paint | < 1.5 seconds | Lighthouse FCP metric |
| Information Display | < 1 second | From crop selection to display |
| Bundle Size | < 200KB | Gzipped JavaScript bundle |
| Memory Usage | < 200MB | During normal operation |

### J.2 Browser Compatibility Test Results

| Browser | Version | Status | Notes |
|---------|---------|--------|-------|
| Chrome | Latest | ✅ Compatible | Full functionality |
| Firefox | Latest | ✅ Compatible | Full functionality |
| Safari | Latest | ✅ Compatible | Full functionality |
| Edge | Latest | ✅ Compatible | Full functionality |
| Chrome Mobile | Latest | ✅ Compatible | Optimized for mobile |
| Safari Mobile | Latest | ✅ Compatible | iOS optimized |

*Note: Actual test results should be documented based on testing performed.*

---

*End of Appendix*

