# 🚀 **Data Preprocessing Plan**  
*Intelligent Failure Prediction and Diagnosis in CI Pipelines *  

## 📋 **Executive Summary**  
Transform 20 TravisTorrent projects (40 CSV files) into a unified, ML-ready dataset with failure type classification for intelligent CI pipeline diagnosis.  

---  

## 🎯 **Phase 0: Setup & Discovery (30 mins)**  
### **Environment Setup**  
- [ ✅ ] Install dependencies (`pandas`, `numpy`, `matplotlib`, `seaborn`)  
- [ ✅ ] Create project structure: `data/`, `plots/`, `processed/`  
- [ ✅ ] Initialize logging for data quality tracking  

### **Data Inventory**  
- [ ✅ ] **Project Discovery**: List all 20 project folders  
- [ ✅ ] **File Validation**: Verify both CSVs exist per project  
- [ ✅ ] **Size Assessment**: Check file sizes and estimated record counts  
- [ ✅ ] **Sample Preview**: Load 2-3 projects to understand schema variations  

**🎯 Success Criteria**: Complete project inventory with size estimates  

---  

## 📂 **Phase 1: Data Integration & Joining (2 hours)**  
### **Per-Project Processing**  
- [ ✅????✅ ] **Iterative Loading**: Process each of 20 projects  
    - Load `repo-data-travis.csv` (build metadata)  
    - Load `buildlog-data-travis.csv` (execution logs)  
    - **Critical Join**: Merge on `build_id` = `tr_build_id`  
    - Add `project_name` column for cross-project analysis  

### **Join Quality Assessment**  
- [ ] **Match Rate Analysis**: Track successful joins vs orphaned records  
- [ ] **Missing Data Patterns**: Document per-project data completeness  
- [ ] **Schema Consistency**: Verify column alignment across projects  

### **Master Dataset Creation**  
- [ ] **Vertical Stacking**: Combine all 20 projects into unified DataFrame  
- [ ] **Duplicate Detection**: Check for cross-project build ID conflicts  
- [ ] **Initial Stats**: Generate combined dataset summary  

**🎯 Success Criteria**: Single dataset with 500K+ records, >90% join success rate  

---  

## 🧼 **Phase 2: Data Cleaning & Standardization (1.5 hours)**  
### **Critical Field Validation**  
- [ ✅ ] **Essential Columns**: Ensure `status`, `duration`, `build_id` completeness  
- [ ✅ ] **Data Type Fixes**: Standardize timestamps, numeric fields, booleans  
- [ ✅ ] **Status Normalization**: Map status variations to standard categories  

### **Missing Data Strategy**  
- [ ✅ ] **Log Fields**: Handle missing test/build log data  
    - Impute with `-1` for counts, `0` for durations  
    - Flag builds with incomplete log parsing  
- [ ✅ ] **Metadata Fields**: Address missing commit/branch information  
- [ ✅ ] **Threshold Decisions**: Drop rows with >50% missing critical features  

### **Outlier Detection**  
- [ ✅ ] **Duration Outliers**: Identify extremely long/short builds  
- [ ✅ ] **Test Count Anomalies**: Flag unrealistic test numbers  
- [ ✅ ] **Cross-Project Consistency**: Check for project-specific data quality issues  

**🎯 Success Criteria**: <10% data loss, consistent schema, documented quality issues  

---  

## 🔧 **Phase 3: Feature Engineering (2 hours)**  
### **Numerical Features**  
- [ ] **Test Metrics**:  
    - `test_failure_rate = tr_log_num_tests_failed / tr_log_num_tests_run`  
    - `test_suite_failure_rate = tr_log_num_test_suites_failed / tr_log_num_test_suites_run`  
    - `test_coverage = tr_log_num_tests_run / max(project_avg_tests)`  

- [ ] **Duration Features**:  
    - `log_duration_ratio = tr_log_testduration / duration`  
    - `setup_time_ratio = tr_log_setup_time / duration`  
    - `build_efficiency = tr_log_buildduration / duration`  

### **Categorical Features**  
- [ ] **Technical Stack**: Extract from `tr_log_frameworks`, `tr_log_lan`  
- [ ] **Build Context**: Process `event_type`, `branch`, `pull_req`  
- [ ] **Project Characteristics**: Add project size, domain, complexity metrics  

### **Temporal Features**  
- [ ] **Time-based Patterns**: Extract from `started_at`  
    - Hour of day, day of week  
    - Build frequency patterns  
    - Time since last build  

**🎯 Success Criteria**: 15-25 engineered features ready for modeling  

---  

## 🎯 **Phase 4: Failure Type Classification (2.5 hours)**  
### **Binary Classification Setup**  
- [ ] **Success/Failure Labels**: Map `status` to binary target  
- [ ] **Failure Rate Analysis**: Calculate per-project failure distributions  

### **Multi-class Failure Classification** *(Your Key Innovation)*  
**For failed builds only (`status` = 'failed' or 'errored'):**  

- [ ] **Code Failures**:  
    - `tr_log_bool_tests_failed = True` + `tr_log_num_tests_failed > 0`  
    - Compilation issues in `tr_log_status`  

- [ ] **Infrastructure Failures**:  
    - `tr_log_analyzer` indicates setup/dependency issues  
    - High `tr_log_setup_time` with no tests run  
    - Network/repository access patterns  

- [ ] **Timeout Failures**:  
    - `duration` exceeds project-specific thresholds  
    - `tr_log_testduration` = 0 but `duration` > median  

- [ ] **Test Failures**:  
    - `tr_log_bool_tests_failed = True` with reasonable `duration`  
    - Specific test failure patterns in logs  

### **Label Validation**  
- [ ] **Confidence Scoring**: Flag ambiguous classifications  
- [ ] **Manual Validation**: Sample 100 cases for accuracy check  
- [ ] **Cross-Project Consistency**: Ensure patterns hold across projects  

**🎯 Success Criteria**: 80%+ of failed builds classified with failure type  

---  

## 📊 **Phase 5: Cross-Project Validation Setup (1 hour)**  
### **Stratified Splitting Strategy**  
- [ ] **Project-wise Splits**: Ensure each project in train/val/test  
- [ ] **Stratification**: Balance by failure types and project characteristics  
- [ ] **Cross-Project Validation**: Reserve 3-4 projects for final testing  

### **Data Partitioning**  
- [ ] **Training Set**: 70% (14 projects)  
- [ ] **Validation Set**: 15% (3 projects)  
- [ ] **Test Set**: 15% (3 projects)  
- [ ] **Project Fold IDs**: Create for k-fold cross-validation  

**🎯 Success Criteria**: Balanced splits ready for ML pipeline  

---  

## 💾 **Phase 6: Output Generation & Documentation (1 hour)**  
### **File Outputs**  
- [ ] `combined_dataset.csv` - Complete unified dataset  
- [ ] `processed_features.csv` - ML-ready features only  
- [ ] `failure_classification.csv` - Failure type analysis  
- [ ] `project_metadata.json` - Per-project statistics  
- [ ] `data_quality_report.json` - Cleaning and join statistics  

### **Visualization Dashboard**  
- [ ] **Failure Distribution**: Across projects and types  
- [ ] **Feature Correlations**: Heatmap of engineered features  
- [ ] **Data Quality**: Missing values, outliers, joins success  
- [ ] **Temporal Patterns**: Build frequency and failure trends  

### **Documentation**  
- [ ] **README**: Data processing pipeline documentation  
- [ ] **Feature Dictionary**: Detailed feature descriptions  
- [ ] **Quality Report**: Data cleaning decisions and statistics  

**🎯 Success Criteria**: Complete documentation and reproducible outputs  

---  

## ⏰ **Time Allocation (8 hours total)**  
| Phase | Duration | Critical Path |  
|-------|----------|--------------|  
| Setup & Discovery | 30 mins | Project inventory |  
| Data Integration | 2 hours | **Join strategy** |  
| Data Cleaning | 1.5 hours | Missing data handling |  
| Feature Engineering | 2 hours | Test/duration metrics |  
| Failure Classification | 2.5 hours | **Multi-class labeling** |  
| Validation Setup | 1 hour | Cross-project splits |  
| Output & Documentation | 1 hour | Final deliverables |  

---  

## 🎯 **Success Metrics for Today**  
- [ ] **Data Completeness**: 500K+ records with <10% loss  
- [ ] **Join Success**: >90% successful merges across projects  
- [ ] **Feature Quality**: 15-25 meaningful features engineered  
- [ ] **Failure Classification**: 80%+ of failures labeled by type  
- [ ] **Cross-Project Validation**: Balanced splits ready  
- [ ] **Documentation**: Complete processing pipeline documented  

---  

## 🚀 **Tomorrow's Handoff**  
- Clean, labeled dataset ready for ML model development  
- Feature importance baseline established  
- Failure pattern documentation for model interpretation  
- Cross-project validation framework in place  

**Ready to start with Phase 0 setup and discovery?**  