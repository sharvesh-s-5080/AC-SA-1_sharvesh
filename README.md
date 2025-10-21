# Skill Assessment Question
 Title: Web-Based Demonstration of Modulation Techniques (AM, FM, DSB-SC, and SSB-SC)

# Problem Statement:
You are required to design and implement an interactive web page that demonstrates four analog modulation techniques used in communication systems:

Amplitude Modulation (AM)

Frequency Modulation (FM)

Double Sideband Suppressed Carrier (DSB-SC)

Single Sideband Suppressed Carrier (SSB-SC)

The web page should allow the user to:

Input carrier frequency (fc), message frequency (fm), and modulation index (where applicable).

Generate and display the corresponding time-domain waveform of each modulation type.

Clearly label axes and provide legends for better visualization.

Switch between AM, FM, DSB-SC, and SSB-SC through user controls (buttons or dropdown menu).

# Requirements & Guidelines:
HTML & CSS

Create a clean, structured webpage layout with appropriate headings.

Provide input boxes for carrier frequency, message frequency, and modulation index.

Style the webpage for readability and user-friendliness.

JavaScript (or Python with Web-based Library, if allowed)

Implement the mathematical equations for each modulation scheme:

AM:
s(t)=[1+μcos⁡(2πfmt)]cos⁡(2πfct)

FM:
s(t)=cos⁡(2πfct+βsin⁡(2πfmt))

DSB-SC:
s(t)=cos⁡(2πfmt)⋅cos⁡(2πfct)

SSB-SC (using Hilbert transform or approximation):
s(t)=cos⁡(2πfct)cos⁡(2πfmt)±sin⁡(2πfct)sin⁡(2πfmt)

Use a plotting library (e.g., Plotly.js, Chart.js, or Canvas API) to plot waveforms dynamically.

Functionality

A "Generate Waveform" button should plot the selected modulation waveform.

The user should be able to switch between modulation techniques easily.

Each waveform must be clearly distinguishable.

Evaluation Criteria

Correctness of implementation of modulation equations.

Clarity of plots (labels, legends, axes).

User interactivity and design.

Code quality (modular, well-commented, readable).

# Deliverables:
A single HTML file (with embedded CSS & JS, or linked external files).

Screenshots showing each modulation waveform for sample inputs.

A short write-up explaining:

The mathematical basis of each modulation type.

How the code implements these equations.

# code:
```
  <!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Analog Modulation Demo</title>
    <script src="https://cdn.plot.ly/plotly-latest.min.js"></script>
    <style>
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            margin: 0;
            padding: 0;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: #333;
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
        }

        .container {
            max-width: 900px;
            width: 100%;
            background: rgba(255, 255, 255, 0.95);
            padding: 30px;
            border-radius: 15px;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.3);
            backdrop-filter: blur(10px);
            margin: 20px;
        }

        h1 {
            text-align: center;
            color: #4a4a4a;
            margin-bottom: 20px;
            font-size: 2.5em;
            text-shadow: 1px 1px 2px rgba(0, 0, 0, 0.1);
        }

        .description {
            text-align: center;
            margin-bottom: 30px;
            font-size: 1.1em;
            color: #666;
        }

        .inputs {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
            gap: 15px;
            margin-bottom: 30px;
        }

        .input-group {
            display: flex;
            flex-direction: column;
        }

        label {
            margin-bottom: 5px;
            font-weight: bold;
            color: #555;
        }

        input,
        select {
            padding: 12px;
            border: 2px solid #ddd;
            border-radius: 8px;
            font-size: 1em;
            transition: border-color 0.3s, box-shadow 0.3s;
        }

        input:focus,
        select:focus {
            border-color: #667eea;
            box-shadow: 0 0 5px rgba(102, 126, 234, 0.5);
            outline: none;
        }

        button {
            padding: 12px 50px;
            margin-left: 400px;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
            border: none;
            border-radius: 8px;
            font-size: 1.1em;
            cursor: pointer;
            transition: transform 0.2s, box-shadow 0.2s;
            grid-column: span 2;
            justify-self: center;
        }

        button:hover {
            transform: translateY(-2px);
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.2);
        }

        #plotDiv {
            width: 100%;
            height: 800px;
            /* Increased height for three subplots */
            border-radius: 10px;
            overflow: hidden;
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.1);
        }

        @media (max-width: 600px) {
            .inputs {
                grid-template-columns: 1fr;
            }

            button {
                grid-column: span 1;
            }
        }
    </style>
</head>

<body>
    <div class="container">
        <h1>Analog Modulation Techniques Demo</h1>
        <p class="description">Explore Amplitude Modulation (AM), Frequency Modulation (FM), Double Sideband Suppressed Carrier (DSB-SC), and Single Sideband Suppressed Carrier (SSB-SC). Adjust parameters and generate waveforms!</p>
        <div class="inputs">
            <div class="input-group">
                <label for="fc">Carrier Frequency (fc) Hz:</label>
                <input type="number" id="fc" value="100">
            </div>
            <div class="input-group">
                <label for="fm">Message Frequency (fm) Hz:</label>
                <input type="number" id="fm" value="10">
            </div>
            <div class="input-group">
                <label for="modIndex">Modulation Index:</label>
                <input type="number" id="modIndex" value="0.5" step="0.1">
            </div>
            <div class="input-group">
                <label for="modType">Modulation Type:</label>
                <select id="modType">
                    <option value="AM">AM</option>
                    <option value="FM">FM</option>
                    <option value="DSB-SC">DSB-SC</option>
                    <option value="SSB-SC">SSB-SC</option>
                </select>
            </div>
            <button onclick="generateWaveform()">Generate Waveform</button>
        </div>
        <div id="plotDiv"></div>
    </div>

    <script>
        function generateWaveform() {
            // Get user inputs
            let fc = parseFloat(document.getElementById('fc').value);
            let fm = parseFloat(document.getElementById('fm').value);
            let modIndex = parseFloat(document.getElementById('modIndex').value);
            let type = document.getElementById('modType').value;

            // Generate time array (0 to 2/fm seconds, 1000 points for smoothness)
            let numPoints = 1000;
            let duration = 2 / fm;
            let t = [];
            let carrierSignal = [];
            let messageSignal = [];
            let modulatedSignal = [];
            for (let i = 0; i < numPoints; i++) {
                t.push(i * duration / numPoints);
                let time = t[i];
                let message = Math.cos(2 * Math.PI * fm * time); // Message signal: cos(2π fm t)
                let carrier = Math.cos(2 * Math.PI * fc * time); // Carrier: cos(2π fc t)

                carrierSignal.push(carrier);
                messageSignal.push(message);

                // Compute modulated signal based on type
                if (type === 'AM') {
                    modulatedSignal.push((1 + modIndex * message) * carrier); // AM equation
                } else if (type === 'FM') {
                    modulatedSignal.push(Math.cos(2 * Math.PI * fc * time + modIndex * Math.sin(2 * Math.PI * fm * time))); // FM equation
                } else if (type === 'DSB-SC') {
                    modulatedSignal.push(message * carrier); // DSB-SC equation
                } else if (type === 'SSB-SC') {
                    // SSB-SC upper sideband: cos(2π fc t) cos(2π fm t) + sin(2π fc t) sin(2π fm t)
                    modulatedSignal.push(carrier * message + Math.sin(2 * Math.PI * fc * time) * Math.sin(2 * Math.PI * fm * time));
                }
            }

            // Create traces for subplots
            let traceCarrier = {
                x: t,
                y: carrierSignal,
                mode: 'lines',
                name: 'Carrier Signal',
                line: {
                    color: '#ff6b6b',
                    width: 2
                },
                xaxis: 'x',
                yaxis: 'y'
            };
            let traceMessage = {
                x: t,
                y: messageSignal,
                mode: 'lines',
                name: 'Message Signal',
                line: {
                    color: '#4ecdc4',
                    width: 2
                },
                xaxis: 'x2',
                yaxis: 'y2'
            };
            let traceModulated = {
                x: t,
                y: modulatedSignal,
                mode: 'lines',
                name: type + ' Modulated Signal',
                line: {
                    color: '#667eea',
                    width: 2
                },
                xaxis: 'x3',
                yaxis: 'y3'
            };

            // Layout with subplots (3 rows, 1 column)
            let layout = {
                grid: {
                    rows: 3,
                    columns: 1,
                    pattern: 'independent'
                },
                title: {
                    text: type + ' Modulation Waveforms',
                    font: {
                        size: 20,
                        color: '#4a4a4a'
                    }
                },
                xaxis: {
                    title: {
                        text: 'Time (s)',
                        font: {
                            size: 14
                        }
                    },
                    gridcolor: '#e0e0e0'
                },
                yaxis: {
                    title: {
                        text: 'Amplitude',
                        font: {
                            size: 14
                        }
                    },
                    gridcolor: '#e0e0e0'
                },
                xaxis2: {
                    title: {
                        text: 'Time (s)',
                        font: {
                            size: 14
                        }
                    },
                    gridcolor: '#e0e0e0'
                },
                yaxis2: {
                    title: {
                        text: 'Amplitude',
                        font: {
                            size: 14
                        }
                    },
                    gridcolor: '#e0e0e0'
                },
                xaxis3: {
                    title: {
                        text: 'Time (s)',
                        font: {
                            size: 14
                        }
                    },
                    gridcolor: '#e0e0e0'
                },
                yaxis3: {
                    title: {
                        text: 'Amplitude',
                        font: {
                            size: 14
                        }
                    },
                    gridcolor: '#e0e0e0'
                },
                showlegend: true,
                plot_bgcolor: 'rgba(255,255,255,0.8)',
                paper_bgcolor: 'rgba(255,255,255,0)'
            };

            // Plot using Plotly with subplots
            Plotly.newPlot('plotDiv', [traceCarrier, traceMessage, traceModulated], layout);
        }
    </script>
</body>

</html>
```

# Output:
   Front Page:
   
   <img width="700" height="700" alt="image" src="https://github.com/user-attachments/assets/cb40eab7-e87d-41a4-937e-d38688403382" />

   

   AM:
   
   <img width="700" height="700" alt="image" src="https://github.com/user-attachments/assets/5d3ce4a9-1ebb-47a2-b5c5-71eb53f0a5b2" />



   FM:
   
   <img width="700" height="700" alt="image" src="https://github.com/user-attachments/assets/0ad3081a-236a-4181-97e0-55f2eaf34e89" />
   


   DSB-SE:
   
   <img width="700" height="700" alt="image" src="https://github.com/user-attachments/assets/db806fa5-ff24-4eb5-b2dd-fe6bc49aa856" />
   


  SSB-SE:
   
   <img width="777" height="970" alt="image" src="https://github.com/user-attachments/assets/f24755d6-082f-437a-89ab-3c4e006ef86a" />






   

