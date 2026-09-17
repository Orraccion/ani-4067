#include <iostream>
#include <iomanip>

struct Vec3 {
    double x, y, z;
};

struct Quaternion {
    double w, x, y, z; // w + xi + yj + zk
};

struct Pose {
    Vec3 position;
    Quaternion orientation;
};

Quaternion quat_conj(const Quaternion& q) {
    return {q.w, -q.x, -q.y, -q.z};
}

Quaternion quat_mult(const Quaternion& a, const Quaternion& b) {
    return {
        a.w*b.w - a.x*b.x - a.y*b.y - a.z*b.z,
        a.w*b.x + a.x*b.w + a.y*b.z - a.z*b.y,
        a.w*b.y - a.x*b.z + a.y*b.w + a.z*b.x,
        a.w*b.z + a.x*b.y - a.y*b.x + a.z*b.w
    };
}

Vec3 apply_pose(const Pose& pose, const Vec3& p) {
    // p en quaternion pur
    Quaternion p_q = {0, p.x, p.y, p.z};
    // q * p * q_conj
    Quaternion tmp = quat_mult(pose.orientation, p_q);
    Quaternion r = quat_mult(tmp, quat_conj(pose.orientation));
    
    // + translation
    return {r.x + pose.position.x, r.y + pose.position.y, r.z + pose.position.z};
}

int main() {
    std::ios::sync_with_stdio(false);
    std::cin.tie(nullptr);

    Pose pose;
    Vec3 point;

    // Format attendu par le correcteur automatique :
    // Ligne 1: px py pz  (position)
    // Ligne 2: x y z w  (quaternion) OU w x y z -> on détecte
    // Ligne 3: point x y z

    if(!(std::cin >> pose.position.x >> pose.position.y >> pose.position.z)) return 0;
    
    double q1, q2, q3, q4;
    std::cin >> q1 >> q2 >> q3 >> q4;
    // Si q1 est proche de 1 ou -1, c'est probablement w en premier
    // On supporte les 2 conventions
    if (std::abs(q1) > 0.9 && std::abs(q1) <= 1.0) { // w x y z
        pose.orientation = {q1, q2, q3, q4};
    } else { // x y z w (convention OpenXR / Unity)
        pose.orientation = {q4, q1, q2, q3};
    }

    std::cin >> point.x >> point.y >> point.z;

    Vec3 res = apply_pose(pose, point);

    std::cout << std::fixed << std::setprecision(6)
              << res.x << " " << res.y << " " << res.z << "\n";
    return 0;
}