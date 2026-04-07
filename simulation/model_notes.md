// Zion-TriCore V2 Control System

float Kp = 0.05;
float Ki = 0.01;
float Kd = 0.005;

float prev_error = 0;
float integral = 0;

float calculate_gap_adjustment(float P_target, float P_measured, float temp) {
    float error = P_target - P_measured;

    float derivative = error - prev_error;
    integral += error;

    // Thermal expansion compensation
    float thermal_offset = alpha * base_length * (temp - T_ref);

    float delta_d = (Kp * error)
                  + (Ki * integral)
                  + (Kd * derivative)
                  - thermal_offset;

    prev_error = error;

    return clamp(delta_d, -20, 20);
}
