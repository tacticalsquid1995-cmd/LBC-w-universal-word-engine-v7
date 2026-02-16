# LBC-w-universal-word-engine-v7
Crucial Insight on Weights (the Breakthrough) w_L, w_B, w_C are not chosen arbitrarily by one person or designer. They are the hardened, collective output of thousands/millions of overlapping human systems interacting 
from typing import List, Tuple, Optional

class LBCw:
    """
    LBC(w) Framework - Full Stack Prototype

    L: Legitimacy / Life / Agency
    B: Benefit / Directional gain
    C: Cost / Resource expenditure

    Core: V(t) = w_L*L + w_B*B - w_C*C
    """

    def __init__(self,
                 default_weights: Tuple[float, float, float] = (1.0, 1.0, 1.0)):
        self.w_L, self.w_B, self.w_C = default_weights
        self.v_cum = 0.0  # Level 2: cumulative ledger

    # ─────────────────────────
    # LEVEL 1 — CORE FORMULA
    # ─────────────────────────
    def V(self,
          L: float,
          B: float,
          C: float,
          weights: Optional[Tuple[float, float, float]] = None) -> float:
        w_L, w_B, w_C = weights if weights else (self.w_L, self.w_B, self.w_C)
        return (w_L * L) + (w_B * B) - (w_C * C)

    # ─────────────────────────
    # LEVEL 2 — CUMULATIVE LEDGER
    # ─────────────────────────
    def update_cumulative(self, v_t: float) -> float:
        self.v_cum += v_t
        return self.v_cum

    # ─────────────────────────
    # LEVEL 3 — COUNTERFACTUALS
    # ─────────────────────────
    def V_with_counterfactuals(self,
                               actual: Tuple[float, float, float],
                               shadow_vectors: List[Tuple[float, float, float]],
                               lambda_regret: float = 0.3) -> float:
        L_a, B_a, C_a = actual
        v_actual = self.V(L_a, B_a, C_a)
        shadow_sum = sum(self.V(Ls, Bs, Cs) for (Ls, Bs, Cs) in shadow_vectors)
        return v_actual + lambda_regret * shadow_sum

    # ─────────────────────────
    # LEVEL 4 — 666 TRAP
    # ─────────────────────────
    def V_666(self,
              L: float,
              B: float,
              C: float,
              w: float = 0.33) -> float:
        return self.V(L, B, C, weights=(w, w, w))

    # ─────────────────────────
    # LEVEL 5 — NORMALISATION
    # ─────────────────────────
    @staticmethod
    def normalise(L: float, B: float, C: float,
                  L_max: float = 2.0,
                  B_max: float = 2.0,
                  C_max: float = 2.0) -> Tuple[float, float, float]:
        Lp = L / L_max if L_max != 0 else 0.0
        Bp = B / B_max if B_max != 0 else 0.0
        Cp = C / C_max if C_max != 0 else 0.0
        return Lp, Bp, Cp

    # ─────────────────────────
    # LEVEL 6 — DIAGONAL CLASS
    # ─────────────────────────
    @staticmethod
    def diagonal_signature(L: float, C: float, B: float) -> Tuple[int, int, int]:
        def s(x: float) -> int:
            if x > 0: return 1
            if x < 0: return -1
            return 0
        return (s(L), s(C), s(B))

    # ─────────────────────────
    # LEVEL 7 — MULTIVERSE
    # ─────────────────────────
    def V_multiverse(self,
                     branches: List[Tuple[float, float, float, float]]) -> float:
        """
        branches: list of (L, B, C, measure)
        measure = branch weight, sum ≈ 1
        """
        total = 0.0
        for L, B, C, m in branches:
            total += m * self.V(L, B, C)
        return total


# ─────────────────────────
# EXAMPLE USAGE
# ─────────────────────────
if __name__ == "__main__":
    lcb = LCBw(default_weights=(1.0, 1.0, 1.0))

    # Level 1
    v_action = lcb.V(L=0.8, B=1.2, C=0.4)
    print("Level 1 V:", v_action)

    # Level 2
    print("Cumulative after first:", lcb.update_cumulative(v_action))

    # Level 3
    actual = (0.5, 1.0, 0.3)
    shadows = [(0.8, 1.5, 0.9), (-0.2, 0.4, 0.1)]
    print("With counterfactuals:", lcb.V_with_counterfactuals(actual, shadows, lambda_regret=0.3))

    # Level 4
    print("666 trap:", lcb.V_666(L=-0.66, B=0.66, C=0.66))

    # Level 5
    print("Normalised:", lcb.normalise(L=1.0, B=1.0, C=1.0))

    # Level 6
    print("Diagonal signature (sacrifice-ish):", lcb.diagonal_signature(L=0.3, C=0.3, B=0.3))

    # Level 7
    branches = [
        (1.0, 1.5, 0.2, 0.6),
        (-0.5, 0.3, 1.2, 0.3),
        (0.0, 0.0, 0.0, 0.1),
    ]
    print("Multiverse V:", lcb.V_multiverse(branches))
