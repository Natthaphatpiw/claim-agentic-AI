# FastAPI Bottle Claim Detection

A specialized media analysis application for **bottle claim detection** and damage assessment, using OpenAI's vision models to evaluate claim eligibility for Chang beer bottles and similar use cases.

## Project Overview

FastAPI Bottle Claim Detection is a web application built to analyze Chang beer bottles and determine if they qualify for claims based on their condition. The application processes uploaded images or videos and generates detailed assessment reports that evaluate the bottle's structural integrity across multiple components (cap, neck, body, bottom). Assessment results are provided in both English and Thai languages.

## Features

- Upload and analyze images (JPG, PNG) or videos (MP4) of bottles
- Support for single or multiple image analysis
- Video frame extraction and detailed analysis
- Proprietary assessment criteria for claim eligibility determination
- Component-based evaluation with individual part scoring
- Percentage-based condition scoring with 80% threshold for claim eligibility
- Detailed visual characteristic identification
- Dual language output (English and Thai)
- Token usage tracking for API consumption monitoring
- User-friendly web interface
- Robust error handling for file type, size, and API errors

## Assessment Process

The application follows a structured evaluation process:
1. **Brand Verification**: Confirms the bottle is Chang brand (bottles of other brands are automatically rejected)
2. **Detailed Component Analysis**:
   - **Cap**: Checks presence and seal integrity
   - **Neck**: Evaluates structural condition and damage
   - **Body**: Assesses completeness and crack patterns
   - **Bottom**: Examines base integrity and separation
3. **Characteristic Matching**: Compares against known patterns of claimable/unclaimable bottles
4. **Score Calculation**: Assigns percentage scores to each component and overall condition
5. **Decision**: Determines claim eligibility based on 80% threshold
6. **Report Generation**: Creates detailed assessment with checkmarks (✅/❌) for passing/failing conditions

## Prerequisites

- Python 3.8+
- OpenAI API key with access to Vision models
- FFmpeg (for video processing)
- OpenCV and NumPy

## Installation

1. Clone the repository:
   ```
   git clone https://github.com/Natthaphatpiw/claim-agentic-AI.git
   
   ```

2. Install the required dependencies:
   ```
   pip install -r requirements.txt
   ```

3. Create a `.env` file in the project root with your OpenAI API key:
   ```
   AZURE_OPENAI_API_KEY=""
   AZURE_OPENAI_ENDPOINT=""
   VISION_MODEL = "gpt-4.1"
   DATE_EXTRACTION_MODEL = "gpt-4.1"
   FALLBACK_VISION_MODEL = "gpt-4o"
   # AZURE_OPENAI_API_VERSION = "2024-12-01-preview" #for gpt-4o
   AZURE_OPENAI_API_VERSION = "2025-03-01-preview" #for gpt-4.1
   
   ```
   **Note:** The `.env` file is required. The application will not start without a valid `AZURE_OPENAI_API_KEY, AZURE_OPENAI_ENDPOINT`.

## Usage

1. Start the server (recommended):
   ```
   uvicorn main:app --reload
   ```
   For Windows users, you can use the provided PowerShell script:
   ```
   .\restart-server.ps1
   ```

2. Open your browser and navigate to:
   ```
   http://localhost:8000
   ```

3. Upload bottle images or videos and receive a detailed assessment report with claim eligibility determination.

## API Endpoints

- `GET /` — Serves the web interface (`static/index.html`)
- `GET /document` — Serves the documentation page (`static/docs.html`)
- `GET /manual` — Serves the user manual page (`static/manual.html`)
- `POST /analyze/` — Processes uploaded media files for bottle assessment

### POST /analyze/

**Request:**
- `files`: List of files (JPG, PNG, MP4)
- `prompt` (optional): Text to guide the analysis (currently not used; system uses predefined prompts)

**Response:**
```json
{
  "english": "Detailed assessment in English with claim/unclaim decision and component scores",
  "thai": "Thai language translation of the assessment",
  "token_usage": {
    "input_tokens": 123,
    "output_tokens": 456,
    "total_tokens": 579
  }
}
```

**Error Handling:**
- Returns clear error messages for unsupported file types, file size limits (10MB per image, 50MB per video), and API errors.
- Only JPG, PNG images and MP4 videos are supported. Uploading other file types or exceeding size limits will result in a descriptive error.

## Key Assessment Criteria

The application evaluates bottles using a comprehensive set of characteristics including:

**For Claimable Bottles:**
- Breakage at specific structural weak points
- Specific fragmentation and crack patterns
- Detached neck with sealed cap integrity
- Main body remaining largely intact
- Visible but contained damage patterns

**For Unclaimable Bottles:**
- Catastrophic fragmentation
- Extensive crack propagation
- Significant structural instability
- Multiple sharp edges and dangerous fragments
- Severe compromise of bottle integrity

## วิธีใช้งานแอปพลิเคชัน (ภาษาไทย)
แอปพลิเคชันนี้ถูกออกแบบมาเพื่อช่วยให้ผู้ใช้สามารถประเมินขวดเบียร์ช้างว่าเข้าเกณฑ์สำหรับการเคลมหรือไม่ โดยใช้ AI วิเคราะห์จากภาพที่ผู้ใช้อัปโหลด

🔄 ขั้นตอนการใช้งาน
เตรียมไฟล์รูปภาพหรือวิดีโอของขวดเบียร์ช้างที่ต้องการประเมิน

แบ่งเป็น 2 ประเภทของภาพที่ต้องอัปโหลด:

(1) รูปขวดเบียร์ที่แสดงฉลากพร้อมวันที่ผลิตอย่างชัดเจน

จำนวน: อย่างน้อย 1 รูป (แนะนำ 1–3 รูป)

รูปควรเห็น วันเดือนปีที่ผลิต ชัดเจนบริเวณฉลากบนขวด

(2) รูปหรือวิดีโอความเสียหายของขวดเบียร์

ใช้สำหรับประเมินว่าความเสียหายนั้นเข้าเกณฑ์การเคลมหรือไม่

รองรับหลายรูป หรือวิดีโอความยาวไม่เกิน 10MB

เข้าสู่หน้าเว็บและอัปโหลดไฟล์

เปิดเบราว์เซอร์และไปที่ http://localhost:8000

เลือกอัปโหลดไฟล์ทั้ง 2 ประเภทตามที่ระบุไว้

กดปุ่ม “ประเมิน” เพื่อเริ่มการวิเคราะห์ด้วย AI

รับผลลัพธ์การประเมิน

ระบบจะแสดงผลลัพธ์ 2 ส่วน:

✅ การตรวจสอบวันผลิต: ขวดผลิตมาแล้ว ไม่เกิน 120 วัน หรือไม่

✅ การประเมินความเสียหาย: ขวดเสียหาย ผ่านเกณฑ์การเคลม หรือไม่

ผลลัพธ์จะมีทั้งภาษาไทยและภาษาอังกฤษ

🛑 หมายเหตุ
ไฟล์ที่รองรับ: .jpg, .png, .mp4

ขนาดไฟล์: ภาพไม่เกิน 10MB, วิดีโอไม่เกิน 50MB

เฉพาะขวดแบรนด์ ช้าง (Chang) เท่านั้น หากตรวจพบว่าไม่ใช่จะถูกปฏิเสธโดยอัตโนมัติ
